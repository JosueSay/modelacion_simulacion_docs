# Problema de Transporte

## ¿Qué es?

Es un problema clásico de **optimización lineal** donde se busca **minimizar el costo total** de transportar bienes desde múltiples fuentes (por ejemplo, fábricas) a múltiples destinos (por ejemplo, puntos de venta), respetando las capacidades de oferta y las necesidades de demanda.

## Estructura del problema

Dada una matriz de costos de transporte $c_{ij}$ donde cada $c_{ij}$ representa el **costo de transportar una unidad del producto desde la fuente $i$ al destino $j$**, se desea determinar las variables $x_{ij}$, que son las **cantidades a transportar desde $i$ hacia $j$**, con el objetivo de minimizar el costo total de transporte.

También tenemos:

- Un **vector de demanda** $d_j$: cantidad de producto requerida en cada destino $j$.
- Un **vector de oferta o suministro** $s_i$: cantidad disponible en cada fuente $i$.

## Formulación del problema

**Función objetivo:**

Minimizar:

$$
\min \sum_{i=1}^m \sum_{j=1}^n c_{ij} x_{ij}
$$

**Sujeto a:**

- Restricción de oferta (cada fuente no puede enviar más de lo que tiene):

$$
\sum_{j=1}^n x_{ij} = s_i \quad \forall i = 1, \dots, m
$$

- Restricción de demanda (cada destino debe recibir su requerimiento exacto):

$$
\sum_{i=1}^m x_{ij} = d_j \quad \forall j = 1, \dots, n
$$

- No negatividad:

$$
x_{ij} \geq 0 \quad \forall i,j
$$

Este problema es **lineal** y se puede resolver con métodos como **Simplex** o herramientas computacionales como **Julia** usando el paquete JuMP.

## Casos posibles

Denotamos:

- $S = \sum_{i=1}^m s_i$: suministro total.
- $D = \sum_{j=1}^n d_j$: demanda total.

Y analizamos tres casos:

## Caso 1: $S = D$ (caso balanceado)

Este es el caso más sencillo. Todo lo que se produce debe ser enviado y cubre exactamente lo que se necesita. Solo hay que decidir cómo distribuir los envíos de forma óptima.

### Ejemplo

**Matriz de costos:**

$$
C = \begin{bmatrix}
8 & 6 & 10 & 9 \\
9 & 10.5 & 13 & 8.5 \\
14 & 9 & 16 & 5 \\
12 & 8 & 7 & 6
\end{bmatrix}
$$

**Suministro:**

$$
s = \begin{bmatrix}
20 \\
30 \\
25 \\
15
\end{bmatrix}
$$

**Demanda:**

$$
d = \begin{bmatrix}
10 \\
35 \\
25 \\
20
\end{bmatrix}
$$

Como $S = D = 90$, todo se puede distribuir sin excedentes ni faltantes.

### Implementación en Julia

```julia
using JuMP, HiGHS

model = Model()

supply = [20, 30, 25, 15]
demand = [10, 35, 25, 20]

costos = [8 6 10 9;
          9 10.5 13 8.5;
          14 9 16 5;
          12 8 7 6]

@variable(model, x[1:4, 1:4] >= 0)

for i in 1:4
    @constraint(model, sum(x[i, j] for j in 1:4) == supply[i])
end

for j in 1:4
    @constraint(model, sum(x[i, j] for i in 1:4) == demand[j])
end

@objective(model, Min, sum(costos[i, j] * x[i, j] for i in 1:4, j in 1:4))

set_optimizer(model, HiGHS.Optimizer)
optimize!(model)
```

**Resultado esperado:**

$$
X = \begin{bmatrix}
0 & 20 & 0 & 0 \\
10 & 10 & 10 & 0 \\
0 & 5 & 0 & 20 \\
0 & 0 & 15 & 0
\end{bmatrix}
$$

Costo total: **695**

Validar que:

- Cada fila sume la oferta correspondiente.
- Cada columna sume la demanda correspondiente.

## Caso 2: $S > D$ (exceso de producción)

¿Qué ocurre si se produce más de lo que se necesita? Se introduce un **destino ficticio** para almacenar el excedente con costo $0$ (o costo de almacenaje si se desea penalizar).

### Nuevos datos

**Nueva demanda:**

$$
d = \begin{bmatrix}
10 \\
25 \\
20 \\
15 \\
20
\end{bmatrix}
$$

**Nuevo costo (columna extra de ceros):**

$$
C = \begin{bmatrix}
8 & 6 & 10 & 9 & 0 \\
9 & 10.5 & 13 & 8.5 & 0 \\
14 & 9 & 16 & 5 & 0 \\
12 & 8 & 7 & 6 & 0
\end{bmatrix}
$$

### Julia

```julia
model2 = Model()

supply = [20, 30, 25, 15]
demand = [10, 25, 20, 15, 20]

costos = [8 6 10 9 0;
          9 10.5 13 8.5 0;
          14 9 16 5 0;
          12 8 7 6 0]

@variable(model2, x[1:4, 1:5] >= 0)

for i in 1:4
    @constraint(model2, sum(x[i, j] for j in 1:5) == supply[i])
end

for j in 1:5
    @constraint(model2, sum(x[i, j] for i in 1:4) == demand[j])
end

@objective(model2, Min, sum(costos[i, j] * x[i, j] for i in 1:4, j in 1:5))

set_optimizer(model2, HiGHS.Optimizer)
optimize!(model2)
```

**Resultado ejemplo:**

$$
X = \begin{bmatrix}
0 & 15 & 5 & 0 & 0 \\
10 & 0 & 0 & 0 & 20 \\
0 & 10 & 0 & 15 & 0 \\
0 & 0 & 15 & 0 & 0
\end{bmatrix}
$$

**Costo total:** 500

Se puede penalizar el almacenamiento modificando la columna extra:

```julia
costos[:,5] = [6, 10, 3, 5]
```

Esto modifica el resultado para favorecer almacenar en la fuente con menor costo de almacenamiento.

## Caso 3: $S < D$ (demanda mayor que la oferta)

Cuando no se produce suficiente, se añade una **fuente ficticia** con costo $0$ (o penalización por no cumplimiento).

### Nuevos datos

**Suministro (nueva fuente):**

$$
s = \begin{bmatrix}
20 \\
30 \\
25 \\
15 \\
15
\end{bmatrix}
$$

**Demanda:**

$$
d = \begin{bmatrix}
25 \\
35 \\
25 \\
20
\end{bmatrix}
$$

**Costo:**

$$
C = \begin{bmatrix}
8 & 6 & 10 & 9 \\
9 & 10.5 & 13 & 8.5 \\
14 & 9 & 16 & 5 \\
12 & 8 & 7 & 6 \\
0 & 0 & 0 & 0
\end{bmatrix}
$$

### Julia

```julia
model3 = Model()

supply = [20, 30, 25, 15, 15]
demand = [25, 35, 25, 20]

costos = [8 6 10 9;
          9 10.5 13 8.5;
          14 9 16 5;
          12 8 7 6;
          0 0 0 0]

@variable(model3, x[1:5, 1:4] >= 0)

for i in 1:5
    @constraint(model3, sum(x[i, j] for j in 1:4) == supply[i])
end

for j in 1:4
    @constraint(model3, sum(x[i, j] for i in 1:5) == demand[j])
end

@objective(model3, Min, sum(costos[i, j] * x[i, j] for i in 1:5, j in 1:4))

set_optimizer(model3, HiGHS.Optimizer)
optimize!(model3)
```

**Resultado ejemplo:**

$$
X = \begin{bmatrix}
0 & 20 & 0 & 0 \\
25 & 5 & 0 & 0 \\
0 & 5 & 0 & 20 \\
0 & 0 & 15 & 0 \\
0 & 5 & 10 & 0
\end{bmatrix}
$$

**Costo total:** 647.5

La última fila representa lo que **no se pudo satisfacer** con producción real, y podría tener un costo asociado como penalización.

## Extensión del problema: Transporte con nodos intermedios

En situaciones más reales, puede existir una **capa intermedia** (por ejemplo, un centro de distribución o "hub") donde los productos no van directamente de fuente a destino.

En este caso, el problema se modela como dos etapas:

1. Transporte desde fuente a hub.
2. Transporte desde hub a destino.

Cada etapa tiene su propia matriz de costos y variables. El objetivo global se vuelve:

$$
\min \left( \sum_{i,h} c^{(1)}_{ih} x^{(1)}_{ih} + \sum_{h,j} c^{(2)}_{hj} x^{(2)}_{hj} \right)
$$

Y se deben agregar restricciones que garanticen **flujo continuo** entre las dos etapas.

Este tipo de problemas se resuelven como un gran modelo lineal con múltiples conjuntos de variables y restricciones, pero el principio de **equilibrar oferta y demanda minimizando el costo** se mantiene.

## Problema de Asignación

### ¿Qué es?

El problema de asignación es una aplicación especial de la programación lineal entera, donde se busca asignar **personas** a **tareas** de manera que se minimice el costo total de la asignación, cumpliendo con las siguientes condiciones:

- Cada persona realiza exactamente **una tarea**.
- Cada tarea debe ser realizada por **una sola persona**.

Esto es muy común en contextos como la asignación de profesores a cursos, empleados a turnos, salones a clases, etc.

### Estructura del problema

Tenemos una matriz de costos $C = [c_{ij}]$, donde:

- $i$ representa una **persona**.
- $j$ representa una **tarea**.
- $c_{ij}$ es el **costo** de asignar la persona $i$ a la tarea $j$.

Definimos una matriz de variables binarias $X = [x_{ij}]$ donde:

$$
x_{ij} =
\begin{cases}
1, & \text{si la persona } i \text{ es asignada a la tarea } j \\
0, & \text{en otro caso}
\end{cases}
$$

### Función objetivo

Minimizar el costo total de asignación:

$$
\min \sum_{i=1}^{m} \sum_{j=1}^{n} c_{ij} x_{ij}
$$

### Restricciones

1. Cada persona realiza exactamente una tarea:

$$
\sum_{j=1}^n x_{ij} = 1 \quad \forall i
$$

2. Cada tarea es realizada por una persona:

$$
\sum_{i=1}^m x_{ij} = 1 \quad \forall j
$$

3. Las variables son binarias:

$$
x_{ij} \in \{0,1\}
$$

### ¿Por qué es útil?

Este modelo asegura que:

- No se asigna a una persona a múltiples tareas.
- Cada tarea se completa.
- El costo total de asignación es mínimo.

También puede pensarse como un **problema de transporte balanceado**, donde cada persona es un proveedor con oferta $1$, y cada tarea es una demanda con requerimiento $1$.

### Casos posibles

Al igual que en el problema de transporte, puede haber tres escenarios:

1. **#Personas = #Tareas** (caso balanceado)
2. **#Personas > #Tareas**
3. **#Personas < #Tareas**

A continuación, se analiza cada uno.

### Caso 1: Número de personas = número de tareas

#### Datos

**Matriz de costos:**

$$
C = \begin{bmatrix}
9 & 2 & 7 & 8 \\
6 & 4 & 3 & 7 \\
5 & 8 & 1 & 8 \\
7 & 6 & 9 & 4
\end{bmatrix}
$$

#### Julia

```julia
using JuMP, HiGHS

model = Model()

costos = [ 9 2 7 8;
           6 4 3 7;
           5 8 1 8;
           7 6 9 4 ]

@variable(model, x[1:4, 1:4], Bin)

for i in 1:4
    @constraint(model, sum(x[i, j] for j in 1:4) == 1)
end

for j in 1:4
    @constraint(model, sum(x[i, j] for i in 1:4) == 1)
end

@objective(model, Min, sum(costos[i, j] * x[i, j] for i in 1:4, j in 1:4))

set_optimizer(model, HiGHS.Optimizer)
optimize!(model)
```

**Resultado:**

$$
X = \begin{bmatrix}
0 & 1 & 0 & 0 \\
1 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 \\
0 & 0 & 0 & 1
\end{bmatrix}
$$

**Costo total:** 13

### Caso 2: Más personas que tareas ($m > n$)

Si hay más personas que tareas, algunas personas no serán asignadas. Se agregan **columnas ficticias** con costo $0$.

#### Datos

**Matriz de costos extendida (5 personas, 4 tareas):**

$$
C = \begin{bmatrix}
9 & 2 & 7 & 8 & 0 \\
6 & 4 & 3 & 7 & 0 \\
5 & 8 & 1 & 8 & 0 \\
7 & 6 & 9 & 4 & 0 \\
5 & 3 & 7 & 6 & 0
\end{bmatrix}
$$

#### Julia

```julia
model1 = Model()

costos = [ 9 2 7 8 0;
           6 4 3 7 0;
           5 8 1 8 0;
           7 6 9 4 0;
           5 3 7 6 0 ]

@variable(model1, x[1:5, 1:5], Bin)

for i in 1:5
    @constraint(model1, sum(x[i, j] for j in 1:5) == 1)
end

for j in 1:4
    @constraint(model1, sum(x[i, j] for i in 1:5) == 1)
end

@objective(model1, Min, sum(costos[i, j] * x[i, j] for i in 1:5, j in 1:5))

set_optimizer(model1, HiGHS.Optimizer)
optimize!(model1)
```

**Resultado:**

$$
X = \begin{bmatrix}
0 & 1 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 1 \\
0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 & 0 \\
1 & 0 & 0 & 0 & 0
\end{bmatrix}
$$

**Costo total:** 12

Una persona queda sin tarea (asignada a columna ficticia).

### Caso 3: Más tareas que personas ($m < n$)

Si hay más tareas que personas, algunas tareas no se podrán asignar. Se agregan **filas ficticias** (personas ficticias) con costo $0$.

#### Datos

**Matriz de costos (4 personas, 5 tareas):**

$$
C = \begin{bmatrix}
9 & 2 & 7 & 8 & 8 \\
6 & 4 & 3 & 7 & 6 \\
5 & 8 & 1 & 8 & 7 \\
7 & 6 & 9 & 4 & 5 \\
0 & 0 & 0 & 0 & 0
\end{bmatrix}
$$

#### Julia

```julia
model2 = Model()

costos = [ 9 2 7 8 8;
           6 4 3 7 6;
           5 8 1 8 7;
           7 6 9 4 5;
           0 0 0 0 0 ]

@variable(model2, x[1:5, 1:5], Bin)

for i in 1:5
    @constraint(model2, sum(x[i, j] for j in 1:5) == 1)
end

for j in 1:5
    @constraint(model2, sum(x[i, j] for i in 1:5) == 1)
end

@objective(model2, Min, sum(costos[i, j] * x[i, j] for i in 1:5, j in 1:5))

set_optimizer(model2, HiGHS.Optimizer)
optimize!(model2)
```

**Resultado:**

$$
X = \begin{bmatrix}
0 & 1 & 0 & 0 & 0 \\
1 & 0 & 0 & 0 & 0 \\
0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 & 0 \\
0 & 0 & 0 & 0 & 1
\end{bmatrix}
$$

**Costo total:** 13

#### ¿Qué pasa si se modifica un costo?

Si cambiamos $c_{4,1} = 4$ en lugar de 7, podría convenir dejar que la tarea 1 la haga una persona ficticia y asignar la tarea 5 a una persona real.

**Resultado nuevo:**

$$
X = \begin{bmatrix}
0 & 1 & 0 & 0 & 0 \\
0 & 0 & 0 & 0 & 1 \\
0 & 0 & 1 & 0 & 0 \\
0 & 0 & 0 & 1 & 0 \\
1 & 0 & 0 & 0 & 0
\end{bmatrix}
$$

### Extensión: Scheduling Problem

Cuando se incluyen más restricciones como:

- Disponibilidad de horarios.
- Preferencias.
- Restricciones de precedencia (una tarea depende de otra).
- Capacidades (salones adaptados, requisitos especiales).

...se convierte en un **problema de programación entera más general** que requiere técnicas más complejas (como restricciones adicionales o incluso heurísticas).
