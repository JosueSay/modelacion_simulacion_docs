# Documentación explicativa sobre Programación Lineal con JuMP y aplicación práctica

Esta documentación tiene como objetivo explicar paso a paso la formulación, implementación y resolución de un problema de **programación lineal**, usando el lenguaje de programación Julia con el paquete `JuMP`, y mostrar cómo modelar un caso práctico de una fábrica que produce diferentes productos. Se hará énfasis en **¿por qué?** y **¿qué significa?** cada paso.

## 1. ¿Qué es la programación lineal?

La **programación lineal (PL)** es una técnica matemática utilizada para optimizar (maximizar o minimizar) una función lineal sujeta a un conjunto de restricciones lineales. Se usa para tomar decisiones óptimas en problemas de recursos limitados.

## 2. Ejemplo básico

### Problema

Maximizar:

$z = 2x_1 + x_2 - 3x_3 + 5x_4$

Sujeto a:

$
\begin{aligned}
x_1 + 2x_2 + 2x_3 + 4x_4 &\leq 40 \\
2x_1 - x_2 + x_3 + 2x_4 &\leq 8 \\
4x_1 - 2x_2 + x_3 - x_4 &\leq 10 \\
x_1, x_2, x_3, x_4 &\geq 0
\end{aligned}
$

### ¿Por qué se hace esto?

Queremos encontrar los valores de $x_1, x_2, x_3, x_4$ que **maximicen la función objetivo** sin violar ninguna de las restricciones.

## 3. Estandarización del problema

Convertimos las desigualdades en igualdades agregando **variables de holgura** ($s_1, s_2, s_3$):

$
\begin{aligned}
x_1 + 2x_2 + 2x_3 + 4x_4 + s_1 &= 40 \\
2x_1 - x_2 + x_3 + 2x_4 + s_2 &= 8 \\
4x_1 - 2x_2 + x_3 - x_4 + s_3 &= 10
\end{aligned}
$

**¿Por qué?**  
El método simplex trabaja con ecuaciones, no desigualdades. Las variables de holgura representan los recursos no utilizados.

## 4. Forma canónica

Se reescribe la función objetivo para usarla en el método simplex:

$z - 2x_1 - x_2 + 3x_3 - 5x_4 = 0$

## 5. Solución y variables básicas

Tras 3 iteraciones del método simplex, se obtiene:

$
\begin{aligned}
x_2 &= 6 \\
x_4 &= 7 \\
x_1 &= 0 \\
x_3 &= 0 \\
z &= 41
\end{aligned}
$

**¿Qué significa que $x_1$ y $x_3$ no aparezcan?**  
Significa que son **variables no básicas** y, por lo tanto, su valor es cero.

## 6. Instalación y uso de JuMP (Julia)

```julia
import Pkg
using Pkg
Pkg.add("JuMP")
Pkg.add("HiGHS")
Pkg.add("Ipopt")
Pkg.add("Optimization")
```

**¿Qué es JuMP?**
Es un paquete de Julia para modelar problemas de optimización matemática.

**¿Qué es un solver?**
Es el algoritmo que encuentra la solución óptima (por ejemplo, HiGHS, Ipopt).

## 7. Ejemplo de uso de JuMP

### Definición de modelo

```julia
model = Model()
```

**¿Por qué está vacío?**
Porque solo se define la estructura general del problema, sin datos aún.

### Definición de variables

```julia
@variable(model, x1 >= 0)
@variable(model, x2 >= 0)
```

### Agregar restricciones

```julia
@constraint(model, 6x1 + 2x2 <= 24)
@constraint(model, x1 + 2x2 <= 6)
@constraint(model, -x1 + x2 <= 1)
@constraint(model, x2 <= 2)
```

### Definir función objetivo

```julia
@objective(model, Max, 5x1 + 4x2)
```

### Configurar y aplicar solver

```julia
set_optimizer(model, HiGHS.Optimizer)
optimize!(model)
```

### Obtener resultados

```julia
println("x1 = ", value(x1))
println("x2 = ", value(x2))
println("z = ", objective_value(model))
```

## 8. Caso práctico: Fábrica con penalización

Una empresa fabrica 4 productos: chamarras ($x_1$), relleno de plumas ($x_2$), pantalones ($x_3$), guantes ($x_4$). Cada uno pasa por 4 etapas (corte, aislamiento, costura, empaque) con tiempo limitado.

### Función objetivo (ganancia)

Se desea **maximizar**:

$$
F = (30x_1 + 40x_2 + 20x_3 + 10x_4) - (15(800 - x_1) + 20(750 - x_2) + 10(600 - x_3) + 8(500 - x_4))
$$

**¿Por qué se penaliza?**
Si no se cumple con la demanda, se pierde una ganancia potencial (penalización).

### Simplificando

$$
F = 45x_1 + 60x_2 + 30x_3 + 18x_4 - 37000
$$

### Restricciones de capacidad

(Etapas con tiempo máximo disponible)

$
\begin{aligned}
0.30x_1 + 0.30x_2 + 0.25x_3 + 0.15x_4 &\leq 1000 \quad \text{(Corte)} \\
0.25x_1 + 0.35x_2 + 0.30x_3 + 0.10x_4 &\leq 1000 \quad \text{(Aislamiento)} \\
0.45x_1 + 0.50x_2 + 0.40x_3 + 0.22x_4 &\leq 1000 \quad \text{(Costura)} \\
0.15x_1 + 0.15x_2 + 0.10x_3 + 0.05x_4 &\leq 1000 \quad \text{(Empaque)}
\end{aligned}
$

### Restricciones de demanda

$
\begin{aligned}
x_1 &\leq 800 \\
x_2 &\leq 750 \\
x_3 &\leq 600 \\
x_4 &\leq 500
\end{aligned}
$

### Restricciones de no negatividad

$
x_1, \; x_2, \; x_3, \; x_4 \geq 0
$

### Solución óptima (continuo)

$
\begin{aligned}
x_1 &= 800 \\
x_2 &= 750 \\
x_3 &= 387.5 \\
x_4 &= 500 \\
\text{ganancia} &= 64{,}625
\end{aligned}
$

## 9. Variables enteras y binarias

En algunos contextos, no se pueden producir fracciones de unidades. Para estos casos se define:

```julia
@variable(model, x1 >= 0, Int)
@variable(model, bandera, Bin)
```

**¿Por qué?**
Porque en la realidad no existen medias chamarras, y a veces se necesitan decisiones binarias (sí/no).

### Cambios al solver

Cuando hay variables enteras o binarias, **ya no se puede usar el método simplex**, sino un solver de programación entera como **MIP**.

### Resultado con enteros

$
\begin{aligned}
x_1 &= 800 \\
x_2 &= 750 \\
x_3 &= 388 \\
x_4 &= 499 \\
\text{ganancia} &= 64{,}622
\end{aligned}
$

### Caso especial: forzar producción

Si se desea **obligar** a que $x_4 = 500$, se impone:

```julia
@constraint(model, x4 == 500)
```

Resultado:

$
\begin{aligned}
x_1 &= 800 \\
x_2 &= 750 \\
x_3 &= 387 \\
x_4 &= 500 \\
\text{ganancia} &= 64{,}610
\end{aligned}
$

**¿Qué significa esto?**
La ganancia bajó porque al forzar la producción de $x_4$, se perdió flexibilidad de producción en otras variables.

## Conclusión

La programación lineal permite modelar situaciones reales donde se busca **optimizar recursos limitados**, tomando en cuenta restricciones. El uso de herramientas como **JuMP en Julia** facilita la formulación, solución y análisis de estos problemas.

Recordar siempre cuestionar:

* ¿Por qué esta restricción?
* ¿Qué representa cada variable?
* ¿Qué implica una solución no entera?
* ¿Vale la pena forzar decisiones?

## Problema de Transporte en Programación Lineal

### ¿Qué es un problema de transporte?

Un **problema de transporte** es un tipo de problema de **programación lineal** en el que se busca determinar la forma más eficiente (por ejemplo, de menor costo) de transportar un producto desde varios **orígenes** (fuentes de producción) hacia varios **destinos** (lugares con demanda), cumpliendo ciertas **restricciones de oferta y demanda**.

### ¿Por qué se estudian estos problemas?

Se aplican en logística, cadenas de suministro y distribución, donde transportar bienes tiene un costo que depende del origen y del destino. La idea es **minimizar el costo total de transporte**, **satisfaciendo toda la demanda** sin exceder la producción de cada fuente.

### Supuestos del modelo

1. Existen $m$ **fuentes** de producción, indexadas como $i = 1, 2, ..., m$.
2. Existen $n$ **destinos** de consumo, indexados como $j = 1, 2, ..., n$.
3. Cada fuente $i$ tiene una **oferta** $s_i$ (cantidad máxima que puede producir).
4. Cada destino $j$ tiene una **demanda** $d_j$ (cantidad requerida).
5. El **costo de transporte** por unidad desde la fuente $i$ al destino $j$ se denota $c_{ij}$.
6. Se **supone** que la **suma total de la oferta es igual a la demanda total**:

$$
\sum_{i=1}^{m} s_i = \sum_{j=1}^{n} d_j
$$

¿Por qué este supuesto? Si no se cumple, el modelo no puede satisfacer simultáneamente todas las restricciones sin sobras o faltantes, lo que requiere ajustes adicionales.

### Matriz de costos y variables de decisión

* Se representa el problema con dos matrices:

1. **Matriz de costos** $[c_{ij}]$, con dimensiones $m \times n$
2. **Matriz de variables** $[x_{ij}]$, donde $x_{ij}$ indica la cantidad de producto que se transporta de la fuente $i$ al destino $j$.

### ¿Qué significa cada restricción?

#### 1. Restricciones de producción (oferta)

La **suma de productos enviados desde cada fuente** no debe exceder su capacidad:

$$
\sum_{j=1}^{n} x_{ij} \leq s_i \quad \text{para } i = 1, 2, ..., m
$$

**¿Por qué?** Porque una fuente no puede enviar más de lo que puede producir.

#### 2. Restricciones de demanda

La **suma de productos que llegan a cada destino** no debe exceder su demanda:

$$
\sum_{i=1}^{m} x_{ij} \leq d_j \quad \text{para } j = 1, 2, ..., n
$$

**¿Por qué?** Porque enviar más de lo requerido genera desperdicio o almacenamiento innecesario.

### Función objetivo

El objetivo es **minimizar el costo total de transporte**:

$$
\min \sum_{i=1}^{m} \sum_{j=1}^{n} c_{ij} x_{ij}
$$

### Representación gráfica

Este problema puede visualizarse como un **grafo dirigido bipartito**, donde:

* Los **nodos del lado izquierdo** son las fuentes.
* Los **nodos del lado derecho** son los destinos.
* Las **aristas** entre ellos representan posibles rutas de transporte, con costos y cantidades transportadas.

### Ejemplo práctico

#### Fuentes de producción

| Fuente     | Capacidad (supply) |
|------------|--------------------|
| LA         | 1000               |
| Detroit    | 1500               |
| New Orleans| 1200               |

#### Destinos y demanda

| Destino | Demanda |
|---------|---------|
| Miami   | 2300    |
| Denver  | 1400    |

Verificamos:  
Oferta total: $1000 + 1500 + 1200 = 3700$  
Demanda total: $2300 + 1400 = 3700$  
**Entonces se cumple**: $S = D$

#### Matriz de costos $[c_{ij}]$

|         | Denver (j=1) | Miami (j=2) |
|---------|--------------|-------------|
| LA (i=1)| 80           | 215         |
| Detroit | 100          | 108         |
| New O.  | 102          | 68          |

### Planteamiento matemático del modelo

#### Variables de decisión

Sea $x_{ij}$ la cantidad enviada de la fuente $i$ al destino $j$.

Total de variables: $m \times n = 3 \times 2 = 6$  
Variables: $x_{11}, x_{12}, x_{21}, x_{22}, x_{31}, x_{32}$

#### Restricciones

##### Producción

$$
\begin{aligned}
x_{11} + x_{12} &= 1000 \\
x_{21} + x_{22} &= 1500 \\
x_{31} + x_{32} &= 1200
\end{aligned}
$$

#### Demanda

$$
\begin{aligned}
x_{11} + x_{21} + x_{31} &= 2300 \\
x_{12} + x_{22} + x_{32} &= 1400
\end{aligned}
$$

**¿Por qué ahora son igualdades?**  
Porque se asume que se usa toda la producción y se satisface completamente la demanda (ya que $S = D$).

#### Función objetivo

$$
\min 80x_{11} + 215x_{12} + 100x_{21} + 108x_{22} + 102x_{31} + 68x_{32}
$$

### Implementación en un modelo de optimización (ejemplo pseudocódigo)

```julia
model = Model()

@variable(model, x11 >= 0)
@variable(model, x12 >= 0)
@variable(model, x21 >= 0)
@variable(model, x22 >= 0)
@variable(model, x31 >= 0)
@variable(model, x32 >= 0)

# Restricciones de oferta
@constraint(model, x11 + x12 == 1000)
@constraint(model, x21 + x22 == 1500)
@constraint(model, x31 + x32 == 1200)

# Restricciones de demanda
@constraint(model, x11 + x21 + x31 == 2300)
@constraint(model, x12 + x22 + x32 == 1400)

# Función objetivo
@objective(model, Min, 80x11 + 215x12 + 100x21 + 108x22 + 102x31 + 68x32)

set_optimizer(model, HiGHS.Optimizer)
```

### Conclusión

El problema de transporte es una forma específica y eficiente de modelar problemas logísticos, aprovechando la estructura del modelo lineal para optimizar costos. Esta formulación puede extenderse a casos más complejos, como múltiples productos, tiempos de envío, restricciones adicionales, etc.
