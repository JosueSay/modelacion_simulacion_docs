# Fundamentos para la optimización unidimensional (1D)

## Unimodalidad

Una función es **unimodal** si dentro de un intervalo cerrado $[a, b]$ presenta **un único mínimo local**, que en este caso también es mínimo global en ese intervalo. Es decir, si aplicamos un algoritmo de optimización, éste siempre convergerá a ese único mínimo.

> **Unimodal** entonces sabemos que hay un mínimo; entonces si aplico un algoritmo sobre eso va a llegar a un único mínimo posible dentro de un intervalo entre $a$ y $b$.

Sin embargo, una función puede no ser unimodal en todo su dominio:

> Obvio la función completa no es unimodal, pero respecto al único intervalo se vuelve unimodal y se trabaja en otro intervalo. Realmente, el unimodal es en un intervalo.

Además, incluso si la función presenta **picos, saltos o discontinuidades**, puede ser considerada unimodal dentro del intervalo si presenta un solo mínimo:

> Las funciones pueden ser unimodales con picos o saltos, pero esto no le interesa \[al algoritmo], incluso si no es continua. Sino el punto donde hay un mínimo, la función puede ser unimodal.

Esto es clave porque algunos algoritmos de optimización exigen esta propiedad:

> Esto es importante porque el algoritmo a ver pide que sea unimodal, un solo mínimo. El proceso es esbozar la gráfica y darse una idea de dónde está el mínimo para aplicarlo.

## Bimodalidad y multimodalidad

Una función es **bimodal** si presenta **dos mínimos locales distintos**. Se extiende a **multimodal** cuando tiene más de dos mínimos locales.

> **Bimodal:** dos puntos locales
> **Multimodal:** múltiples mínimos o máximos locales

En estos casos, los algoritmos de búsqueda de mínimos pueden **converger a diferentes resultados** dependiendo del punto de inicio o del intervalo seleccionado. Por eso se busca acotar la función a un intervalo donde sea unimodal.

## Algoritmos de optimización 1D

A diferencia de métodos para encontrar ceros de funciones, los algoritmos de optimización 1D buscan **el mínimo** de una función en un intervalo dado.

> En lugar de darme los ceros, me darán el mínimo de la función con un intervalo de trabajo o un punto inicial. Dependiendo del algoritmo puede que pidamos que sea unimodal, diferenciable o que tenga su derivada.

Se estudian métodos que permiten obtener estos mínimos eficientemente, sin calcular derivadas (en algunos casos) ni hacer búsquedas exhaustivas.

## Receta general para métodos de optimización 1D

Para aplicar cualquier método de optimización unidimensional se necesita:

1. **Intervalo o punto inicial:** Indica dónde se buscará el mínimo.
2. **Ecuación de actualización:** Define cómo se generan nuevos puntos.
3. **Criterio de paro:** Por número máximo de iteraciones o tolerancia.

> Esto trata para meter algoritmos de funciones para saber estos mínimos...

## Método de la Razón Áurea (Golden Section Search)

### ¿Por qué "razón áurea"?

> Por ejemplo, $(1 + \sqrt{5}) / 2 \approx 1.618$. Tienen que ver con la proporción áurea, un valor relacionado al Renacimiento, Fibonacci, etc.
> Esto se llama así porque en una parte del algoritmo aparece este número.

### Objetivo

Minimizar una función $f: [a, b] \to \mathbb{R}$, que sea **unimodal** en $[a, b]$.

### Esquema del algoritmo

> Si no lo es \[unimodal], nuestro trabajo es acortar el intervalo donde sí lo sea.

Se parte del intervalo inicial $[a_0, b_0]$ y se escogen dos nuevos puntos:

$$
a_1 = c, \quad b_1 = d
$$

> La condición es que debe haber simetría entre la distancia $a_0 - a_1$ y $b_0 - b_1$, es decir, simétrico en esas distancias.

Estos puntos se colocan usando una proporción $p < \frac{1}{2}$, típicamente:

$$
p = \frac{\sqrt{5} - 1}{2}
$$

De modo que:

$$
c = b - (b - a)\cdot p, \quad d = a + (b - a)\cdot p
$$

Luego:

* Si $f(c) < f(d)$, el mínimo está en $[a, d]$
* Si $f(c) > f(d)$, el mínimo está en $[c, b]$

> Estos dos nuevos puntos lo que me indican es elegir un nuevo lugar de trabajo más pequeño que el original.

### Algoritmo en pseudocódigo

```bash
Algoritmo: (Golden search)
Inputs: [a₀, b₀]: intervalo de búsqueda.
Outputs: x mínimo global de f en [a₀, b₀].

For k = 0, 1, 2, ... hasta que se cumpla criterio de paro:
    cₖ = bₖ − (bₖ − aₖ)(√5−1)/2  
    dₖ = aₖ + (bₖ − aₖ)(√5−1)/2

    xₖ₊₁ = ½(cₖ + dₖ)

    If f(cₖ) < f(dₖ):
        aₖ₊₁ = aₖ, bₖ₊₁ = dₖ
    Else:
        aₖ₊₁ = cₖ, bₖ₊₁ = bₖ

Return xₖ₊₁
```

### Código en Python

```python
def goldenSearch(f, a, b, maxI, tol=1e-5, prt=0):
    """
    Golden-section search optimization method, to find the minimum
    of f on [a,b], f: strictly unimodal function on [a,b]

    Inputs: 
        f    = objective function, strictly unimodal function on [a,b],
        a,b  = interval onto search,
        maxI = maximum number of iterations,
        tol  = tolerance (1e-5 default).

    Outputs:
        x    = location of minimal point,
        y    = f-value at x,
        xk   = sequence of iterates,
        yk   = sequence of f-values a iterates,
        err  = sequence of absolute errors |x_k - x_{k-1}|,
        iters = number of iterations,
        conv  = flag: 1 = convergence, 0 = not convergence (stop by maxI attained).
    """

    gr = (1. + np.sqrt(5.)) / 2.   # golden ratio
    c = b - (b - a) / gr
    d = a + (b - a) / gr

    xk = []
    yk = []
    err = []

    iters = 0
    fin = 0
    conv = 0

    xold = 0

    # main iteration
    while (not fin):
        iters = iters + 1

        # define new interval of search
        if (f(c) < f(d)): 
            b = d
        else: 
            a = c

        # recompute both c and d
        c = b - (b - a) / gr
        d = a + (b - a) / gr

        if (abs(b - a) < tol):
            fin = 1
            conv = 1

        if (iters >= maxI): 
            fin = 1

        x = (b + a) / 2
        y = f(x)

        if (prt == 1):
            print('iter = {} --- x = {} --- f(x) = {}'.format(iters, x, y))

        xk.append(x)
        yk.append(y)
        err.append(abs(x - xold))
        xold = x

    return x, y, xk, yk, err, iters, conv
```

## Interpolación Parabólica

### Motivación

Supongamos una parábola:

$$
f(x) = ax^2 + bx + c
$$

El mínimo de esa parábola (si $a > 0$) está en:

$$
x = -\frac{b}{2a}
$$

> Esto nos sirve para entender el segundo algoritmo "Interpolación parabólica".

### ¿Cómo se construye la parábola?

> Le doy tres puntos: $x_0$, $x_1$, $x_2$ y sus respectivos $y_i = f(x_i)$.
> Entonces construimos una parábola que pase por esos tres puntos.

Una función cuadrática es suficiente para pasar por tres puntos distintos. Si esos puntos son cercanos al mínimo, entonces **la parábola se aproxima a la función** cerca del mínimo real.

### Sistema de ecuaciones lineal

Buscamos los coeficientes $a_k$, $b_k$, $c_k$ de la parábola:

$$
f(x) = a_k x^2 + b_k x + c_k
$$

Dado:

$$
(x_k, f(x_k)), \quad (x_{k+1}, f(x_{k+1})), \quad (x_{k+2}, f(x_{k+2}))
$$

El sistema se representa como:

$$
\begin{pmatrix}
1 & x_k     & x_k^2     \\
1 & x_{k+1} & x_{k+1}^2 \\
1 & x_{k+2} & x_{k+2}^2
\end{pmatrix}
\begin{pmatrix}
c_k \\
b_k \\
a_k
\end{pmatrix}
=
\begin{pmatrix}
f(x_k) \\
f(x_{k+1}) \\
f(x_{k+2})
\end{pmatrix}
$$

### Alternativa: Fórmulas explícitas

> O simplemente se pueden quemar las fórmulas:

$$
\begin{aligned}
c_k &= \frac{(x_{k+1} - x_{k+2})f(x_k) + (x_{k+2} - x_k)f(x_{k+1}) + (x_k - x_{k+1})f(x_{k+2})}
{(x_k - x_{k+1})(x_{k+1} - x_{k+2})(x_{k+2} - x_k)} \\
\\
b_k &= \frac{(x_{k+1}^2 - x_{k+2}^2)f(x_k) + (x_{k+2}^2 - x_k^2)f(x_{k+1}) + (x_k^2 - x_{k+1}^2)f(x_{k+2})}
{(x_k - x_{k+1})(x_{k+1} - x_{k+2})(x_{k+2} - x_k)} \\
\\
a_k &= \frac{(x_{k+1}x_{k+2}^2 - x_{k+2}x_{k+1}^2)f(x_k) + (x_{k+2}x_k^2 - x_kx_{k+2}^2)f(x_{k+1}) + (x_kx_{k+1}^2 - x_{k+1}x_k^2)f(x_{k+2})}
{(x_k - x_{k+1})(x_{k+1} - x_{k+2})(x_{k+2} - x_k)}
\end{aligned}
$$

### Validación

Antes de calcular el nuevo $x = -b_k / 2a_k$, es fundamental verificar que:

> $a_k > 0$. Si no, entonces el mínimo no está bien definido (la parábola se abre hacia abajo) → se debe lanzar error o cambiar puntos.

### Código en Python

```python
def minOfParabola(x0, x1, x2, y0, y1, y2):
    """
    Find the minimal point of a parabola by least squares.
    """
    num = (y0 *(x1**2 - x2**2) +
           y1* (x2**2 - x0**2) +
           y2 * (x0**2 - x1**2))

    den = 2. * (y0 * (x1 - x2) +
                y1 * (x2 - x0) +
                y2 * (x0 - x1))
    
    x = num / den
    return x
```

```python
def parabolicInterpolation(f, a, b, maxI, tol=1e-5, prt=0):
    """
    Parabolic-interpolation search optimization method, to find the minimum
    of f on [a,b], f: strictly unimodal function on [a,b]

    Inputs:
        f     = objective function, strictly unimodal on [a,b],
        a, b  = interval for search,
        maxI  = maximum number of iterations,
        tol   = tolerance (default 1e-5),
        prt   = print flag (1 = print iteration info).

    Outputs:
        x     = location of minimum,
        y     = f(x),
        xk    = list of iterates,
        yk    = list of function values,
        err   = list of absolute errors |x_k - x_{k-1}|,
        iters = number of iterations performed,
        conv  = convergence flag (1 = converged, 0 = not converged).
    """
    # Initial 3 points
    x0 = a
    x1 = b
    x2 = (a + b) / 2   # third point in the middle of [a,b]

    xk = []
    yk = []
    err = []

    y2 = f(x2)
    xk.append(x2)
    yk.append(y2)
    err.append(abs(x2 - x1))

    iters = 0
    fin = 0
    conv = 0

    # Main iteration loop
    while (not fin):
        iters += 1

        # Define new minimal point using interpolation
        y0, y1, y2 = f(x0), f(x1), f(x2)
        x3 = minOfParabola(x0, x1, x2, y0, y1, y2)
        y3 = f(x3)

        # Convergence checks
        if abs(x2 - x1) < tol:
            fin = 1
            conv = 1

        if iters >= maxI:
            fin = 1

        # Print iteration info
        if prt == 1:
            print('iter = {} --- x = {} --- f(x) = {}'.format(iters, x3, y3))

        xk.append(x3)
        yk.append(y3)
        err.append(abs(x3 - x2))

        # Update points for next iteration
        x0 = x1
        x1 = x2
        x2 = x3

    return x3, y3, xk, yk, err, iters, conv
```

## Método de Newton para Optimización

### Fundamento

> Recordando Newton para ceros:

$$
x_{k+1} = x_k - \frac{f(x_k)}{f'(x_k)}
$$

Ahora queremos encontrar el **mínimo** de $f$, lo cual ocurre cuando $f'(x) = 0$. Entonces, buscamos ceros de la derivada:

> Entonces si quiero el mínimo $f'(x) = 0$, es decir, $x$ es mínimo de $f$, se debe cumplir lo anterior.

Aplicamos Newton **a la derivada**:

$$
x_{k+1} = x_k - \frac{f'(x_k)}{f''(x_k)}
$$

Este método converge rápidamente cuando se cumplen las condiciones necesarias, pero:

> Tendremos las mismas desventajas de Newton: derivada igual a cero, ciclos, divergencia, etc.

### Observación

> Sirve también para encontrar **máximos** si el segundo derivado es negativo, pero aquí lo usamos para mínimos.

El resto del código y estructura ya lo habías puesto completo y está bien.

### Código en Python

```python
def optNewton(f, df, ddf, a, b, maxI, tol=1e-5, prt=0):
    """
    Newton search optimization method, to find the minimum
    of f on [a,b], f: strictly unimodal function on [a,b]

    Inputs:
        f     = objective function,
        df    = derivative of f,
        ddf   = second derivative of f,
        a, b  = interval onto search,
        maxI  = maximum number of iterations,
        tol   = tolerance (1e-5 default),
        prt   = print flag (1 = print iteration info).

    Outputs:
        x     = location of minimal point,
        y     = f-value at x,
        xk    = sequence of iterates,
        yk    = sequence of f-values,
        err   = sequence of errors |x_k - x_{k-1}|,
        iters = number of iterations,
        conv  = convergence flag (1 = converged, 0 = not converged)
    """
    x = (a + b) / 2   # middle point of [a,b] as starting point

    xk = []
    yk = []
    err = []

    y = f(x)
    xk.append(x)
    yk.append(y)
    err.append(abs(x - a))
    xold = x

    iters = 0
    fin = 0
    conv = 0

    # main iteration
    while not fin:
        iters += 1

        # define new minimal point using Newton step
        x = x - df(x) / ddf(x)
        y = f(x)

        if abs(x - xold) < tol:
            fin = 1
            conv = 1

        if iters >= maxI:
            fin = 1

        if prt == 1:
            print('iter = {} --- x = {} --- f(x) = {}'.format(iters, x, y))

        xk.append(x)
        yk.append(y)
        err.append(abs(x - xold))
        xold = x

    return x, y, xk, yk, err, iters, conv
```
