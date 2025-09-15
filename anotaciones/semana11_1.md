# Relación entre ecuaciones diferenciales y campos vectoriales

Recordemos que una **ecuación diferencial ordinaria** puede representarse como un **campo vectorial**, y viceversa.
Un campo vectorial asocia a cada punto $(x,y)$ un vector cuya dirección y magnitud están determinadas por la función $f(x,y)$.
De este modo, el análisis de ecuaciones diferenciales se puede visualizar mediante el campo vectorial que generan.

A estos tipos de campos añadiremos dos herramientas útiles para analizar soluciones de ecuaciones diferenciales:

1. El **método de las isóclinas**.
2. El **campo de pendientes**.

En este documento trabajaremos principalmente el **método de las isóclinas**.

## Método de las isóclinas

El método de las isóclinas consiste en trazar curvas en el plano donde la pendiente de las soluciones de la ecuación diferencial es constante.
Partimos de una ecuación de la forma:

$$
\frac{dy}{dx} = f(x,y).
$$

Una **isóclina** es el conjunto de puntos $(x,y)$ donde $f(x,y) = c$, con $c \in \mathbb{R}$ una constante.
De esta manera, sobre cada curva de isóclina, la pendiente de las soluciones diferenciales es la misma.

Estas curvas nos sirven como guía para construir el campo de pendientes y, posteriormente, interpretar el comportamiento de las soluciones de la ecuación diferencial.

### Ejemplo 1

Consideremos la ecuación diferencial:

$$
\frac{dy}{dx} = x - y.
$$

Las isóclinas se obtienen imponiendo:

$$
x - y = c.
$$

Despejando $y$:

$$
y = x - c.
$$

Esto corresponde a una **familia de rectas con pendiente 1** y ordenada al origen variable según $c$.

* Para $c=0$: $y = x$.
* Para $c=1$: $y = x - 1$.
* Para $c=2$: $y = x - 2$.

Cada recta representa una isóclina con pendiente constante. Al graficarlas, sobre cada línea se dibujan pequeños vectores con pendiente $c$. Al observar el conjunto, se visualiza el comportamiento general de las soluciones de la ecuación diferencial.

### Ejemplo 2

Sea la ecuación diferencial:

$$
\frac{dy}{dx} = 3y.
$$

La condición para las isóclinas es:

$$
3y = c \quad \Rightarrow \quad y = \frac{c}{3}.
$$

Esto representa **rectas horizontales** en el plano, cada una a una altura distinta según $c$.

* Para $c=0$: $y=0$, que es una recta horizontal en el eje $x$.
* Para $c=3$: $y=1$.
* Para $c=6$: $y=2$.

Sobre cada recta, todos los puntos tienen la misma pendiente $c$, de manera que se pueden trazar minivectores paralelos. Al repetir esto para distintos valores de $c$, se obtiene una representación aproximada del campo de pendientes.

### Ejemplo 3

Consideremos la ecuación diferencial:

$$
\frac{dy}{dx} = -\frac{x}{y}.
$$

Para determinar las isóclinas, planteamos:

$$
-\frac{x}{y} = c.
$$

Multiplicando ambos lados:

$$
-x = yc \quad \Rightarrow \quad y = -\frac{x}{c}.
$$

Esto corresponde a una familia de **rectas que pasan por el origen**, cuya pendiente depende del parámetro $c$.

* Para $c=1$: $y = -x$.
* Para $c=2$: $y = -\tfrac{x}{2}$.
* Para $c=-1$: $y = x$.

Al graficar estas rectas, se observa que las soluciones de la ecuación diferencial están asociadas a **curvas cerradas** (círculos), dado que la ecuación diferencial original proviene de derivadas relacionadas con trayectorias circulares.

## Diagramas de fase

Un **diagrama de fase** es una representación cualitativa del comportamiento de una ecuación diferencial ordinaria (EDO). Su objetivo es mostrar cómo evolucionan las soluciones sin necesidad de resolver la ecuación de manera explícita.

### Ecuaciones autónomas

Una ecuación diferencial ordinaria se denomina **autónoma** cuando la variable independiente no aparece de forma explícita en la ecuación.

* Forma general de una EDO:

$$
F(x, y, y') = 0
$$

donde:

* $x$ = variable independiente,
* $y = y(x)$ = variable dependiente,
* $y' = \frac{dy}{dx}$ = derivada de $y$ respecto a $x$.

En una **ecuación autónoma**, $x$ no aparece explícitamente:

$$
F(y, y') = 0
$$

Esto no significa que $x$ no participe, sino que su influencia está implícita a través de la relación $y = y(x)$.

Si se cambia la notación de $y(x)$ a $x(t)$, con $t$ como variable independiente:

$$
F(t, x, x') = 0
$$

donde:

* $t$ = variable independiente (por ejemplo, tiempo),
* $x = x(t)$ = variable dependiente,
* $x' = \frac{dx}{dt}$.

En el caso autónomo:

$$
F(x, x') = 0
$$

### Ejemplos

1. $y' = y - x \quad \rightarrow$ **no es autónoma** (depende explícitamente de $x$).
2. $y' = y + y^2 \quad \rightarrow$ **es autónoma** (solo depende de $y$).
3. $\dfrac{dx}{dy} = x + e^t \quad \rightarrow$ **no es autónoma** (aparece $t$).
4. $\dfrac{dx}{dt} = x(x-5) \quad \rightarrow$ **es autónoma** (solo depende de $x$).

### Diagramas de fase en ecuaciones autónomas

En una ecuación autónoma, se pueden identificar **puntos de equilibrio**, es decir, valores donde la derivada se anula:

$$
\frac{dy}{dx} = 0
$$

Cuando la derivada es nula, la variable dependiente alcanza un valor constante:

* No cambia con respecto a $x$ (o $t$).
* Corresponde a una solución **constante**.

Así, los puntos de equilibrio permiten analizar el comportamiento de las soluciones sin resolver la EDO explícitamente.

Ejemplo:

$$
\frac{dy}{dx} = y(y-5)
$$

Los puntos de equilibrio se encuentran resolviendo:

$$
y(y-5) = 0 \quad \Rightarrow \quad y = 0 \quad \text{y} \quad y = 5
$$

### Clasificación de los puntos de equilibrio

1. **Repulsor (inestable):**

   * Las soluciones **se alejan** del punto de equilibrio.
   * El punto "expulsa" las trayectorias cercanas.

2. **Atractor (estable):**

   * Las soluciones **se acercan** al punto de equilibrio.
   * El punto "atrae" las trayectorias cercanas.

3. **Semiestable:**

   * Presentan un comportamiento mixto: algunas trayectorias se acercan mientras que otras se alejan.

### Ejemplo 1

$$
\frac{dy}{dx} = y(y-5)
$$

Puntos de equilibrio:

$$
y = 0, \quad y = 5
$$

Tabla de signos:

| Intervalo       | Valor prueba | Signo de $f(y)$ | Dirección |
| --------------- | ------------ | ----------------- | --------- |
| $(-\infty,0)$ | $y=-1$     | $+$             | arriba    |
| $(0,5)$       | $y=1$      | $-$             | abajo     |
| $(5,\infty)$  | $y=6$      | $+$             | arriba    |

Al graficar:

* En $y=0$: es un **atractor** (las soluciones tienden hacia esta recta).
* En $y=5$: es un **repulsor** (las soluciones se alejan de esta recta).

Este análisis muestra que entre $0$ y $5$ las trayectorias bajan, mientras que fuera de este intervalo las trayectorias suben.

**Nota importante:** antes de realizar el diagrama de fase, conviene verificar condiciones como:

* Continuidad de la función $f(y)$,
* Existencia y unicidad de soluciones.

Esto garantiza que las trayectorias no se cruzan ni se superponen, lo cual da validez al análisis.

### Ejemplo 2 – Crecimiento poblacional

Supongamos una población $P(t)$ que sigue la EDO:

$$
\frac{dP}{dt} = 0.001P(0.2 - 0.5P)
$$

Puntos de equilibrio:

$$
0.001P(0.2 - 0.5P) = 0 \quad \Rightarrow \quad P = 0 \quad \text{o} \quad P = 0.4
$$

Tabla de signos:

| Intervalo        | Valor prueba | Signo de $f(P)$ | Dirección |
| ---------------- | ------------ | ----------------- | --------- |
| $(-\infty,0)$  | $P=-1$     | $-$             | abajo     |
| $(0,0.4)$      | $P=0.2$    | $+$             | arriba    |
| $(0.4,\infty)$ | $P=1$      | $-$             | abajo     |

Interpretación:

* $P=0$: repulsor (la población se aleja del cero).
* $P=0.4$: atractor (la población tiende a estabilizarse en este valor).

Este tipo de modelo es típico en ecología para describir poblaciones con capacidad de carga.

### Ejemplo 3 – Punto semiestable

$$
\frac{dy}{dt} = (y+3)(y-1)^2
$$

Puntos de equilibrio:

$$
y=-3, \quad y=1
$$

Tabla de signos:

| Intervalo        | Valor prueba | Signo de $f(y)$ | Dirección |
| ---------------- | ------------ | ----------------- | --------- |
| $(-\infty,-3)$ | $y=-4$     | $-$             | abajo     |
| $(-3,1)$       | $y=0$      | $+$             | arriba    |
| $(1,\infty)$   | $y=2$      | $+$             | arriba    |

Interpretación:

* En $y=-3$: repulsor (las soluciones se alejan).
* En $y=1$: semiestable (algunas soluciones se acercan desde la izquierda, pero todas crecen hacia la derecha).

### Conclusión

Los diagramas de fase son herramientas cualitativas que permiten:

* Identificar puntos de equilibrio,
* Clasificar su estabilidad,
* Predecir la evolución de las soluciones sin resolver explícitamente la EDO.

Esto resulta útil en aplicaciones como dinámica poblacional, física y modelos de optimización, donde se busca comprender la **tendencia general** del sistema más que su solución exacta.
