# Algoritmos Iterativos

Los algoritmos iterativos son herramientas fundamentales en métodos numéricos. Su objetivo principal es **encontrar aproximaciones** para soluciones de ecuaciones u optimizaciones, incluso cuando no se pueden resolver analíticamente.

Cuando se tiene una **función continua**, es posible que deseemos encontrar sus **mínimos**, **máximos**, o **ceros**. Por ejemplo, si una función se cruza con el eje $x$ en dos puntos, deberíamos poder encontrar estos puntos de cruce. Si la función es un polinomio, existen técnicas algebraicas como la **división sintética** o la **factorización**, pero aquí nos enfocaremos en **técnicas algorítmicas**, que no dependen de álgebra explícita, sino de procesos computacionales repetitivos.

## ¿Qué caracteriza a un algoritmo iterativo?

Un algoritmo iterativo se basa en tres ingredientes fundamentales:

### 1. Punto o puntos iniciales

Este es el valor desde el cual comenzará el algoritmo. Puede ser:

* Un punto inicial único $x_0$
* Una secuencia de puntos iniciales $(x_0, x_1, \dots, x_k)$
* Un intervalo inicial $(a, b)$

Este es el **primer ingrediente esencial**: determinar **dónde comenzar** el proceso.

### 2. Ecuación de actualización

El algoritmo genera una secuencia de aproximaciones:

$$
x_1, x_2, x_3, \dots, x_k
$$

El objetivo es que esta secuencia **converja** a un valor $x^*$ que sea la solución deseada. Para esto se define una **fórmula iterativa** que indica cómo obtener el siguiente valor en la secuencia:

$$
x_{k+1} = F(x_k)
$$

A veces esta actualización puede depender de más de un punto anterior, pero siempre se debe **saber cómo calcular el siguiente punto**.

### 3. Criterio de paro

Es necesario establecer una condición que **indique cuándo detenerse**. De lo contrario, el algoritmo seguiría indefinidamente, consumiendo recursos como memoria o energía.

Uno de los criterios más comunes es:

$$
\|x_{k+1} - x_k\| < \text{tolerancia}
$$

Esto evalúa si el cambio entre dos iteraciones consecutivas es suficientemente pequeño. La **tolerancia** puede ser del orden de $10^{-3}$, $10^{-6}$, etc., lo cual indica cuántas cifras decimales son confiables.

Otros posibles criterios de paro incluyen:

* **Número máximo de iteraciones**
* **Tiempo de ejecución** (menos común en este contexto, pero útil en algoritmos de cómputo intensivo)

> Si alguno de estos tres ingredientes falta, el algoritmo no funcionará correctamente.

## Aplicación: Encontrar ceros de funciones

Veremos tres algoritmos iterativos para **hallar raíces** o **ceros** de funciones. Esto significa encontrar $x$ tal que $f(x) = 0$.

### Método de Bisección

Este método parte de una función continua $f: [a, b] \to \mathbb{R}$, y **requiere un cambio de signo** entre los extremos del intervalo:

$$
f(a) \cdot f(b) < 0
$$

Esto garantiza que existe al menos un cero en el intervalo (por el Teorema del Valor Intermedio).

#### ¿Cómo funciona?

1. Calcular el punto medio:

$$
x = \frac{a + b}{2}
$$

2. Evaluar $f(x)$. Luego, determinar en cuál subintervalo persiste el cambio de signo:

* Si $f(x) \cdot f(a) < 0$, entonces el nuevo intervalo es $[a, x]$
* De lo contrario, se actualiza $a = x$

Este procedimiento se repite hasta que se cumple un **criterio de paro**, como:

$$
|a - b| < \text{tolerancia}
$$

#### Parámetros del algoritmo

* Función $f$
* Intervalo inicial $[a, b]$
* Tolerancia deseada
* Criterio de paro (por tolerancia o número de iteraciones)

> Nota: Si se usa $10^{-t}$ como tolerancia, se esperan $t$ cifras decimales confiables en la solución.

#### ¿Y si hay múltiples ceros?

Se puede ejecutar el algoritmo en **diferentes subintervalos**, explorando el dominio de la función para hallar múltiples raíces.

### Método de la Secante

Este método no parte de un intervalo, sino de **dos puntos iniciales** $x_0$ y $x_1$. Se construye una **recta secante** entre estos puntos, y se encuentra el punto en el que esta recta **corta al eje $x$**, lo que proporciona una nueva aproximación $x_2$.

> Antes de aplicar la fórmula se debe hacer una verificación porque este algoritmo falla cuando los son el mismo,  es decir que $f(x_k-1) == f(x_2)$

#### ¿Cómo se actualiza?

Dado $x_k$ y $x_{k-1}$, se calculan:

* $y_k = f(x_k)$
* $y_{k-1} = f(x_{k-1})$

Entonces, el siguiente punto se obtiene con:

$$
x_{k+1} = x_k - y_k \cdot \frac{x_k - x_{k-1}}{y_k - y_{k-1}}
$$

También puede escribirse como:

$$
x_{k+1} = \frac{x_{k-1} \cdot y_k - x_k \cdot y_{k-1}}{y_k - y_{k-1}}
$$

#### Requisitos

* $x_k \neq x_{k-1}$
* $y_k \neq y_{k-1}$

Esto asegura que la pendiente de la recta secante no sea cero.

#### Criterio de paro

Igual que antes:

$$
\|x_k - x_{k-1}\| < \text{tolerancia}
$$

Este método suele **converger más rápido** que el de bisección, ya que se basa en la geometría de la función.

### Método de Newton-Raphson

Este es uno de los métodos más rápidos y populares, desarrollado en el siglo XVII. A diferencia del método de la secante, este utiliza la **recta tangente** a la función.

#### Requisitos

* Función $f: \mathbb{R} \to \mathbb{R}$ **diferenciable**
* Punto inicial $x_0$

#### ¿Cómo se calcula?

A partir de la fórmula de la recta tangente:

$$
y - f(x_k) = f'(x_k)(x - x_k)
$$

Para encontrar el corte con el eje $x$, se hace $y = 0$ y se despeja:

$$
x_{k+1} = x_k - \frac{f(x_k)}{f'(x_k)}
$$

Esta es la **ecuación de actualización** del método.

#### Condiciones importantes

* $f'(x_k) \neq 0$: la derivada no debe anularse, pues implicaría una tangente horizontal.
* La función debe ser **suavemente diferenciable** en el entorno del punto inicial.

> Al igual que antes se debe verificar que $f'(x_k) \neq 0$

#### Criterio de paro

También se usa:

$$
\|x_{k+1} - x_k\| < \text{tolerancia}
$$

> Aunque este método converge rápidamente, depende críticamente de la derivada. Si no se puede derivar o si la derivada se anula, **no se puede aplicar** este método.

#### ¿Qué pasa con funciones en varias dimensiones?

El método de bisección **no se puede aplicar** directamente en $\mathbb{R}^n$, pero los otros dos métodos pueden adaptarse utilizando conceptos como el **gradiente** y la **norma** para evaluar el criterio de paro:

$$
\|\mathbf{x}_{k+1} - \mathbf{x}_k\| < \text{tolerancia}
$$
