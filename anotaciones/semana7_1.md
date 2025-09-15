# Optimización en $\mathbb{R}^n$

En el contexto de la optimización en espacios multidimensionales ($\mathbb{R}^n$), buscamos encontrar **mínimos locales o globales** de una función dada. A partir de un **punto inicial**, la idea es **avanzar en la dirección de descenso** para reducir el valor de la función, hasta alcanzar un punto donde ya no sea posible mejorar (el mínimo).

## Descenso por Gradiente

La técnica del **descenso por gradiente** parte de un punto inicial $x_0$ y se basa en la información del **gradiente de la función** para determinar hacia dónde moverse.

* **Gradiente:** El gradiente de una función en un punto nos indica la dirección de **máximo ascenso**. Por lo tanto, su **negativo** ($-\nabla f$) apunta en la dirección de **máximo descenso**.

* **Idea clave:** Si empezamos en $x_0$ y calculamos el **negativo del gradiente** en ese punto, obtenemos la dirección en la que la función disminuye más rápido.

Para avanzar, multiplicamos esa dirección por una **longitud de paso** ($\alpha$), que indica **cuánto nos movemos**:

$$
\alpha > 0 \quad \Rightarrow \quad \text{Tamaño de paso}
$$

Este parámetro ($\alpha$) se puede ajustar para **aumentar o disminuir la velocidad** del avance.

La ecuación para actualizar la posición es:

$$
x_1 = x_0 - \alpha \nabla f(x_0)
$$

Y en forma general:

$$
x_{k+1} = x_k - \alpha \nabla f(x_k)
$$

### ¿Cómo saber si vamos en la dirección correcta?

En teoría, quisiéramos que en cada iteración se cumpla:

$$
f(x_{k+1}) < f(x_k)
$$

Es decir, que el valor de la función disminuya. Sin embargo, en la práctica **esto no siempre sucede**, porque:

* Puede que el tamaño de paso $\alpha$ sea demasiado grande, haciendo que saltemos sobre el mínimo.
* Podemos entrar en una región de la función donde el gradiente nos lleva hacia arriba en lugar de bajar.
* La forma de la función (curvatura, oscilaciones) puede afectar el avance.

En **Deep Learning**, este parámetro $\alpha$ se conoce como **learning rate**. Un valor mal elegido puede provocar que el entrenamiento **no converja** o que lo haga de forma extremadamente lenta.

### Uso de direcciones de descenso

En lugar de usar siempre el negativo del gradiente, podemos usar cualquier **dirección de descenso** $d_k$ que cumpla:

$$
f(x_k + t d_k) < f(x_k) \quad \forall t \in (0, T)
$$

Es decir, cualquier dirección que **reduzca el valor de la función**. El gradiente negativo es solo una de ellas.

#### Verificación de dirección de descenso

Podemos comprobar si una dirección es de descenso usando el **producto punto**:

$$
\langle \nabla f(x_k), d_k \rangle < 0 \quad \Rightarrow \quad d_k \text{ es dirección de descenso}
$$

Si el producto es positivo, la dirección apunta hacia arriba y no es útil.

### Relación con el ángulo del gradiente

* Si el ángulo entre $-\nabla f(x_k)$ y $d_k$ es **menor de 90°**, es una dirección válida de descenso.
* Si es **mayor de 90°**, estamos yendo hacia una dirección que **incrementa** la función.

El conjunto de direcciones de descenso forma un **abanico** alrededor del $-\nabla f$.

## Pseudocódigo general

**Requisitos:**

* La función $f$ debe ser diferenciable ($C^1$).
* Se debe contar con una forma de calcular su gradiente $\nabla f$.

**Inputs:**

* $f$ (función objetivo)
* $\nabla f$ (gradiente)
* $\alpha$ (tamaño de paso)
* $x_0$ (punto inicial)
* Criterio de paro (tolerancia, máximo de iteraciones, etc.)

**Versión general:**

1. Elegir una dirección de descenso $d_k$.
2. Actualizar:

   $$
   x_{k+1} = x_k + \alpha d_k
   $$
3. Repetir hasta que se cumpla el criterio de paro.

### Algoritmo: Descenso Gradiente (versión naïve)

**Inputs:** $f : \mathbb{R}^n \to \mathbb{R}$, $x_0 \in \mathbb{R}^n$, $\alpha > 0$.
**Outputs:** $x$ (punto crítico de $f$).

Para $k = 0, 1, 2, \dots$ hasta que se cumpla un criterio de paro:

1. Calcular $d_k$ tal que $\angle(-\nabla f(x_k), d_k) < \frac{\pi}{2}$.
2. Actualizar: $x_{k+1} = x_k + \alpha d_k$.

### Algoritmo: Steepest Descent (versión naïve)

Es un caso particular donde $d_k = -\nabla f(x_k)$:

$$
x_{k+1} = x_k - \alpha \nabla f(x_k)
$$

## Criterios de paro

Algunos criterios comunes para detener el algoritmo son:

1. **Error absoluto en $x$:**

   $$
   \|x_k - x_{k+1}\| < \text{tol}
   $$
2. **Error relativo en $x$:**

   $$
   \frac{\|x_k - x_{k+1}\|}{\|x_k\|} < \text{tol}
   $$
3. **Error absoluto en $f(x)$:**

   $$
   |f(x_k) - f(x_{k+1})| < \text{tol}
   $$
4. **Norma del gradiente:**

   $$
   \|\nabla f(x_k)\| < \text{tol}
   $$

También es recomendable incluir un **límite máximo de iteraciones** para evitar bucles infinitos.

## Elección del tamaño de paso ($\alpha$)

* Si $\alpha$ es muy grande → el método puede **divergir**.
* Si $\alpha$ es muy pequeño → el método **converge** pero demasiado lento.
* Lo ideal es encontrar un $\alpha$ que permita converger **rápido y de forma estable**.

**Estrategia básica para elegir $\alpha$:**

1. Probar un valor inicial $\alpha > 0$.
2. Si no converge, probar $\alpha / 2$ y repetir.
3. Continuar reduciendo (o aumentando) hasta encontrar un valor estable.
4. Ajustar con múltiplos de $0.1$ o potencias de 10 pequeñas.

En muchos algoritmos modernos, $\alpha$ **no es constante**, sino que se adapta dinámicamente (ej. *line search*, *learning rate decay*).

Perfecto, aquí te extiendo y mejoro todo lo que escribiste, sin eliminar nada y respetando tu estructura, pero ampliando cada concepto, explicando parámetros, razones, posibles problemas y agregando aclaraciones para que, al leerlo, no queden dudas. También incluyo el pseudocódigo que mencionaste, resaltando su significado y la lógica detrás.

## Implementaciones en Python y estrategias en $\mathbb{R}^2$ y $\mathbb{R}^n$

En esta sección, analizaremos distintas **implementaciones de descenso por gradiente** y sus variantes. Todas estas estrategias se pueden programar en Python y adaptarse según la función objetivo $f: \mathbb{R}^n \to \mathbb{R}$.

### Primera estrategia: dirección a cierto ángulo del gradiente negativo

En $\mathbb{R}^2$ (y en general en $\mathbb{R}^n$), el vector **$-\nabla f$** nos da la dirección de máximo descenso. Sin embargo, no siempre queremos movernos estrictamente en esa dirección, ya que puede ser útil explorar otras rutas.

* **Idea:** encontrar una dirección que forme un cierto **ángulo** $\theta$ con el gradiente negativo.
* **Restricción:** este ángulo debe ser menor o igual a $90^\circ$ para que la dirección siga siendo de descenso.
* **Parámetro adicional:** el ángulo $\theta$ es configurable. Si sobrepasamos $90^\circ$, ya no estaremos descendiendo.

**¿Por qué puede servir movernos en un ángulo diferente al gradiente negativo?**

* Evita caer directamente en mínimos locales poco profundos.
* Permite explorar el terreno de la función de manera más flexible.
* Puede combinarse con estrategias de aleatoriedad.

### Segunda estrategia: algoritmo de descenso máximo

En este caso, usamos el algoritmo de **Steepest Descent** (descenso más pronunciado):

$$
x_{k+1} = x_k - \alpha \nabla f(x_k)
$$

* **Ventaja:** es simple y directo.
* **Desventaja:** puede oscilar en zonas de curvatura complicada o converger lentamente.

### Tercera estrategia: dirección aleatoria controlada

Otra opción es **introducir aleatoriedad** en la selección de la dirección de descenso:

* Elegir un ángulo $\theta$ aleatorio tal que:

  $$
  0 < \theta \le 90^\circ
  $$
* El vector elegido siempre estará dentro del abanico de direcciones de descenso permitido.
* Esto ayuda a explorar más ampliamente el espacio y escapar de mínimos locales.

### Problemas comunes de estos métodos

Estas implementaciones pueden fallar por varias razones:

1. **No convergencia:**
   El algoritmo puede desviarse y no acercarse a ningún mínimo si $\alpha$ es inadecuado.
2. **Estancamiento en mínimos locales:**
   Una vez que se entra en un mínimo local, el algoritmo puede quedarse atrapado.
3. **Búsqueda del mínimo absoluto:**
   Si la función tiene muchos mínimos locales, encontrar el mínimo absoluto es complicado; no desesperarse si no se logra, ya que estos algoritmos no están diseñados exclusivamente para eso.

### Estrategias para encontrar múltiples mínimos

Cuando la función tiene **varios mínimos**, un solo punto inicial dará como resultado **un único mínimo encontrado**. Para localizar otros mínimos, se puede variar el punto inicial $x_0$.

Dos estrategias principales:

1. **Búsqueda en malla (grid search):**

   * Crear una **rejilla de puntos** en el dominio de la función.
   * Ejecutar el algoritmo desde cada punto inicial.
   * Útil cuando el dominio es pequeño y se puede cubrir bien con pocos puntos.
   * Ejemplo: en 2D, usar una cuadrícula de $m \times m$ puntos y lanzar el algoritmo desde cada uno.

2. **Selección aleatoria de puntos iniciales:**

   * Elegir $N$ puntos al azar dentro del dominio permitido.
   * Lanzar el algoritmo desde cada punto.
   * Más escalable que el grid search en dimensiones altas.

### Estrategia de Cauchy para elegir $\alpha$

En vez de usar un $\alpha$ constante (método naïve), podemos calcular un $\alpha_k$ **óptimo en cada iteración**:

$$
\alpha_k = \arg\min_{t \ge 0} f(x_k + t d_k)
$$

#### Explicación

1. Partimos del punto actual $x_k$ y su dirección de descenso $d_k$.
2. Consideramos la función unidimensional:

   $$
   \varphi(t) = f(x_k + t d_k)
   $$
3. Buscamos el valor $t = \alpha_k$ que minimice $\varphi(t)$.

**Importante:**

* Este mínimo **no es** el mínimo de la función original, sino el mínimo de la “rodaja” en la dirección $d_k$.
* Este proceso puede usar algoritmos unidimensionales como:

  * **Búsqueda dorada** (*Golden Section Search*).
  * **Método de Newton parabólico**.
  * **Bisección**.

**Ventajas:**

* Adapta $\alpha$ a la forma local de la función.
* Mejora la convergencia.

**Desventajas:**

* Más costoso computacionalmente porque requiere una optimización adicional en cada paso.

#### Pseudocódigo: Descenso gradiente con esquema de Cauchy

```bash
Algoritmo: Descenso gradiente (esquema de Cauchy)
Inputs:
    f: función de clase C¹
    x0: punto inicial en R^n
Outputs:
    x: punto crítico de f

k = 0
Mientras no se cumpla el criterio de paro:
    1. Definir d_k = -∇f(x_k)  (o cualquier dirección de descenso)
    2. Calcular α_k = argmin_{t ≥ 0} f(x_k + t d_k) usando búsqueda unidimensional
    3. Actualizar x_{k+1} = x_k + α_k d_k
    4. k = k + 1
Retornar x_{k+1}
```

### Variante con Newton

En el **descenso de Newton**, la dirección de descenso se calcula usando la matriz Hessiana:

$$
d_k = -\left[ \nabla^2 f(x_k) \right]^{-1} \nabla f(x_k)
$$

La actualización es:

$$
x_{k+1} = x_k + \alpha_k d_k
$$

**Ventaja:**

* Puede converger mucho más rápido si la función es bien condicionada.

**Desventaja:**

* Requiere calcular la **segunda derivada** (Hessiana), lo cual es costoso.

### Gradientes conjugados

El método de **gradientes conjugados** es una mejora sobre el descenso de Newton o el gradiente simple:

* En lugar de usar solo $-\nabla f$, la dirección $d_k$ combina información de iteraciones anteriores.
* Incluye un factor $\beta$ que ajusta la dirección según la curvatura de la función.
* Versiones conocidas:

  * Fletcher-Reeves.
  * Polak-Ribiere.

### Métodos cuasi-Newton

Son aproximaciones al método de Newton que **evitan calcular la Hessiana exacta**:

* **BFGS:** actualiza una aproximación de la inversa de la Hessiana.
* **DFP (Davidon-Fletcher-Powell):** similar, pero con otra fórmula de actualización.

#### Algoritmo BFGS

```bash
Require: x0, H0
Ensure: x*
k = 0
Mientras ||∇f_k|| ≠ 0:
    d_k = -H_k ∇f_k
    α_k = búsqueda en línea
    x_{k+1} = x_k + α_k d_k
    Calcular ∇f_{k+1}, y_k, s_k, ρ_k
    Actualizar:
    H_{k+1} = (I - ρ_k s_k y_k^T) H_k (I - ρ_k y_k s_k^T) + ρ_k s_k s_k^T
    k = k + 1
Fin mientras
```

#### Algoritmo DFP

```bash
Require: x0, H0
Ensure: x*
k = 0
Mientras ||∇f_k|| ≠ 0:
    d_k = -H_k ∇f_k
    α_k = búsqueda en línea
    x_{k+1} = x_k + α_k d_k
    Calcular ∇f_{k+1}, y_k, s_k
    Actualizar:
    H_{k+1} = H_k + (s_k s_k^T)/(s_k^T y_k) - (H_k y_k y_k^T H_k)/(y_k^T H_k y_k)
    k = k + 1
Fin mientras
```

### Caso estocástico (SGD)

En **aprendizaje automático**, el gradiente puede calcularse sobre todo el dataset, pero esto es costoso. En su lugar:

* Se divide el conjunto de datos en **batches** de tamaño `batch_size`.
* Se calcula el gradiente solo sobre cada batch.
* Se actualiza la posición después de cada batch.

**Ventajas:**

* Mucho más rápido.
* Introduce una aleatoriedad útil para evitar mínimos locales.

**Aleatoriedad:**

* Proviene del reordenamiento (*shuffle*) de los datos antes de formar los batches.
* Esto crea caminos de descenso más variados.

## Del descenso por gradiente “básico” a variantes avanzadas y su versión estocástica en Deep Learning

En este apartado se explica cómo el descenso por gradiente tradicional —que podría verse como la versión más básica o “de iniciación”— sirve de base para algoritmos más avanzados que mejoran su velocidad y capacidad de convergencia. En el ámbito del *Deep Learning*, además, se introduce una componente **estocástica** no en la elección de direcciones arbitrarias, sino en la forma de calcular el gradiente a partir de los datos.

En problemas reales, la matriz de datos $X$ tiene dimensiones $n \times d$, donde:

* $n$ = número de observaciones o ejemplos.
* $d$ = número de características por observación.

En la versión determinista, el cálculo del gradiente de la función de error $E(n)$ se hace evaluando **todas las observaciones** en cada iteración, lo cual puede ser muy costoso computacionalmente.

El método **estocástico** propone trabajar por lotes (*mini-batches*):

1. Dividir los datos en grupos más pequeños $B_1, B_2, \dots$.
2. Escoger un tamaño de lote (*batch size*).
3. Calcular el gradiente solo con el lote actual, por ejemplo $E_1$ con $B_1$.
4. Actualizar los parámetros del modelo y pasar al siguiente lote.
5. Antes de cada época, reordenar aleatoriamente los datos (*shuffle*) para evitar patrones fijos.

Este enfoque produce **gradientes aproximados** pero más rápidos de calcular, permitiendo:

* Avances más frecuentes en la optimización.
* Menor riesgo de atascarse en mínimos locales.
* Un balance entre velocidad y precisión.

La **aleatoriedad** en este contexto se refiere exclusivamente al reordenamiento de los datos antes de formar los *mini-batches*, no a tomar direcciones de descenso al azar.
