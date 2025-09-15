# Repaso Ecuaciones diferenciables

Son funciones que dependen de una variable regularmente el tiempo, tenemos una variable independiente y dependiente.

Con las ecuaciones diferenciables medimos los cambios. Sirven como modelos, para representar situraciones donde una variable cambia:

- Estudiar cómo cambia la y (v.d.) en el tiempo.
- Estudiar la dinámica de la variable y (v.d.)

Ejemplos:

- Trayectoria de un objeto.
- Fuerzas, deformaciones, estructuras
- Biologúia: dinámica de poblaciones, depredador-presa.
- Economía: equilibrio de mercado, inventario, ventas, etc.
- EDP: modelos de difusión, ecuación de calor, propagación de virus, etc.

Podemos tener varias variables independientes (ecuaciones diferenciales parciales) pero para este curso no trabajaremos mucho con esto.

## Notación de derivadas

Las derivadas pueden escribirse en diferentes formatos, entre los más comunes:

1. **Notación de Leibniz:**

   $$
   \frac{dy}{dx}, \quad \frac{\partial u}{\partial t}
   $$

2. **Notación de Newton:**
   Usada principalmente en física (tiempo como variable).

   $$
   \dot{y}, \quad \ddot{y}
   $$

3. **Notación de Lagrange:**
   Usando apóstrofos.

   $$
   y', \; y'', \; y^{(n)}
   $$

4. **Notación de operador diferencial:**

   $$
   D_x y, \quad D^n y
   $$

### Estándar a usar en el curso

> (Se completará más adelante según lo que se indique en clase)

## Clasificación de ecuaciones diferenciales

Las **ecuaciones diferenciales** describen relaciones entre una función desconocida y sus derivadas. Según el tipo de derivadas que aparezcan, se clasifican en:

- **Ecuaciones diferenciales ordinarias (EDO):**
  Aparecen únicamente derivadas respecto a **una variable independiente**.
  Ejemplo:

  $$
  \frac{dy}{dx} = f(x,y)
  $$

- **Ecuaciones diferenciales parciales (EDP):**
  Involucran **derivadas parciales**, es decir, derivadas respecto a **más de una variable independiente**.
  Ejemplo:

  $$
  \frac{\partial u}{\partial t} = \alpha \Delta u
  $$

### Ejemplos clásicos de EDP

1. **Ecuación de Poisson:**

   $$
   \Delta u = h(x,y,z)
   $$

2. **Ecuación del calor:**

   $$
   \alpha^2 \Delta u + h(x,y,z) = \frac{\partial u}{\partial t}
   $$

3. **Ecuación de onda:**

   $$
   c^2 \Delta u + h(x,y,z) = \frac{\partial^2 u}{\partial t^2}
   $$

4. **Ecuaciones de Navier–Stokes:**

   $$
   \rho\left(\frac{\partial \vec{v}}{\partial t} + \vec{v}\cdot\nabla \vec{v}\right) = -\nabla p + \nabla \cdot T + f
   $$

## Forma general de una ecuación diferencial

Una ecuación diferencial ordinaria de orden $n$ se puede escribir en **forma general** como:

$$
F(x, y, y', y'', \ldots, y^{(n)}) = 0
$$

- $x$: variable independiente.
- $y$: variable dependiente.
- $y', y'', \ldots, y^{(n)}$: derivadas sucesivas de $y$ respecto a $x$.
- $F$: representa una función que combina a $x, y$ y sus derivadas.

👉 Cuando veamos una ecuación escrita así, significa que está en su **forma más general**, donde todo está contenido en una sola expresión.

- **Forma general:**

  $$
  F(t, y, y') = 0
  $$

  Aquí todo está expresado en una sola ecuación que combina a la variable independiente $t$, la dependiente $y$, y su derivada.

- **Forma normal (explícita):**

  $$
  \frac{dy}{dt} = f(t, y)
  $$

  En esta forma la derivada queda despejada, lo cual facilita el análisis y la resolución.

### Clasificación por orden

El **orden** de una ecuación diferencial es el de la **derivada de mayor grado** que aparece en la ecuación.

- Se parece al “exponente” en un polinomio, pero aquí en lugar de potencias, lo que cuenta es **hasta qué derivada llega la ecuación**.

#### Ejemplos

- $\frac{dy}{dx} + 5y = \cos x$ → **1er orden**
- $x''' - t^2x' + \sqrt{t}x = 1$ → **3er orden**
- $\alpha^2\left(\frac{\partial^4 u}{\partial x^4} + \frac{\partial^4 u}{\partial y^4}\right) = \frac{\partial u}{\partial t}$ → **4to orden**

### Sistemas de ecuaciones diferenciales ordinarias (EDO)

Un sistema de EDOs ocurre cuando hay **más de una variable dependiente**, todas relacionadas entre sí.

$$
\begin{aligned}
x_1' &= f_1(t, x_1, x_2, \ldots, x_n) \\
x_2' &= f_2(t, x_1, x_2, \ldots, x_n) \\
&\;\;\vdots \\
x_n' &= f_n(t, x_1, x_2, \ldots, x_n)
\end{aligned}
$$

## Sistema en forma vectorial

El sistema que tenías:

$$
\begin{cases}
x_1' = 5t - 4 + x_3 \\
x_2' = \cos(t) + \sin(2t) + 8 \\
x_3' = 1 + x_1 - x_2
\end{cases}
$$

lo podemos reescribir en **forma vectorial compacta**:

$$
\mathbf{x}'(t) = \mathbf{f}(t,\mathbf{x}),
$$

donde

$$
\mathbf{x}(t) =
\begin{bmatrix}
x_1 \\ x_2 \\ x_3
\end{bmatrix},
\quad
\mathbf{f}(t,\mathbf{x}) =
\begin{bmatrix}
5t - 4 + x_3 \\
\cos(t) + \sin(2t) + 8 \\
1 + x_1 - x_2
\end{bmatrix}.
$$

### ¿Por qué es útil?

1. **Se guarda toda la información en una sola ecuación:**
   En vez de manejar tres ecuaciones distintas, trabajamos con una sola relación vectorial.

2. **Facilita la notación y la programación:**

   - En computación numérica, algoritmos como **Euler, Runge–Kutta, métodos implícitos, etc.** requieren precisamente esta forma:
     $\mathbf{x}' = \mathbf{f}(t,\mathbf{x})$.
   - Esto permite escribir el código una sola vez para cualquier dimensión del sistema.

3. **Generalización inmediata:**

   - El ejemplo se dio en $\mathbb{R}^3$, pero lo mismo funciona en $\mathbb{R}^n$.
   - El tamaño del sistema no importa: siempre podemos escribirlo como una **ecuación vectorial**.

4. **Estructura matemática clara:**

   - Se ve de forma natural que $\mathbf{f}:\mathbb{R}\times\mathbb{R}^n\to\mathbb{R}^n$.
   - Esto permite aplicar resultados teóricos de existencia, unicidad y estabilidad.

5. **Separación en parte lineal y no lineal:**
   El sistema puede descomponerse en

   $$
   \mathbf{x}' = A\,\mathbf{x} + \mathbf{g}(t),
   $$

   con una matriz $A$ que captura la interacción entre variables y una función $\mathbf{g}(t)$ que captura lo que depende solo del tiempo.
   Esto ayuda a:

   - Identificar si el sistema es lineal o no.
   - Usar métodos especiales para sistemas lineales con coeficientes constantes.

## Reducción de una EDO de orden superior a un sistema de primer orden

- Una **ecuación diferencial ordinaria (EDO) de orden $n$** siempre puede transformarse en un **sistema de $n$ ecuaciones de primer orden**.
- Esto significa que **no importa el orden original**, basta con dominar cómo resolver sistemas de **primer orden**.
- En la práctica (y en la computación numérica), **siempre se hace esta reducción** para poder aplicar algoritmos estándar como Runge–Kutta.

### Ejemplo: EDO de 2º orden

Supongamos:

$$
y'' + 3y' + 2y = 0
$$

1. Definimos nuevas variables para bajar el orden:

   - Sea $x_1 = y$
   - Sea $x_2 = y'$

2. Escribimos sus derivadas:

   - $x_1' = y' = x_2$
   - $x_2' = y'' = -3y' - 2y = -3x_2 - 2x_1$

3. Sistema equivalente de primer orden:

$$
\begin{cases}
x_1' = x_2 \\
x_2' = -3x_2 - 2x_1
\end{cases}
$$

o en forma vectorial:

$$
\mathbf{x}' =
\begin{bmatrix}
x_1' \\ x_2'
\end{bmatrix}
=
\begin{bmatrix}
x_2 \\ -3x_2 - 2x_1
\end{bmatrix}.
$$

### Ejemplo: EDO de 5º orden

Si tenemos:

$$
y^{(5)} = f(t, y, y', y'', y^{(3)}, y^{(4)}),
$$

definimos:

$$
x_1 = y, \quad x_2 = y', \quad x_3 = y'', \quad x_4 = y^{(3)}, \quad x_5 = y^{(4)}.
$$

Entonces:

$$
\begin{cases}
x_1' = x_2 \\
x_2' = x_3 \\
x_3' = x_4 \\
x_4' = x_5 \\
x_5' = f(t, x_1, x_2, x_3, x_4, x_5)
\end{cases}
$$

### Ventajas del desacoplamiento

1. **Uniformidad:**
   No importa si la ecuación es de orden 2, 5 o 100, **todas se reducen a un sistema de primer orden**.

2. **Computacional:**
   Algoritmos numéricos estándar (Euler, Runge–Kutta, etc.) están diseñados para sistemas de primer orden.
   → No necesitamos algoritmos distintos para cada orden.

3. **Flexibilidad:**
   El paso de reducción funciona incluso para ecuaciones no lineales.

4. **Interpretación geométrica:**
   El sistema equivalente es siempre una **ecuación vectorial en $\mathbb{R}^n$**.

   - La trayectoria de la solución se interpreta como una **curva en un espacio de dimensión $n$**.

### Ejemplo concreto

Dada la EDO:

$$
x''' - 4x\,x'' + 7x' - 8x = t + e^t
$$

1. **Aislamos la derivada de mayor orden:**

$$
x''' = t + e^t + 4x\,x'' - 7x' + 8x
$$

2. **Definimos nuevas variables para bajar el orden:**

$$
x_1 = x, \quad x_2 = x', \quad x_3 = x''
$$

3. **Escribimos el sistema equivalente de primer orden:**

$$
\begin{cases}
x_1' = x_2 \\
x_2' = x_3 \\
x_3' = t + e^t + 4x_1x_3 - 7x_2 + 8x_1
\end{cases}
$$

4. **Forma vectorial compacta:**

$$
\mathbf{x}(t) =
\begin{bmatrix}
x_1 \\ x_2 \\ x_3
\end{bmatrix}, \qquad
\mathbf{x}'(t) =
\begin{bmatrix}
x_2 \\
x_3 \\
t + e^t + 4x_1x_3 - 7x_2 + 8x_1
\end{bmatrix}.
$$

## Problema de Valor Inicial (PVI)

Un **problema de valor inicial** se escribe como:

$$
x'(t) = f(t, x), \qquad x(t_0) = x_0
$$

- La ecuación diferencial $x' = f(t,x)$ define **la pendiente de la curva solución** en cada punto $(t,x)$.
- La condición inicial $x(t_0) = x_0$ fija **un punto específico por el que debe pasar la solución**.

### Interpretación geométrica

1. **Ejes:**

   - Eje horizontal: el tiempo $t$.
   - Eje vertical: la variable dependiente $x(t)$.

2. **Campo de pendientes:**

   - La función $f(t,x)$ asigna una pendiente a cada punto del plano $(t,x)$.
   - Esto se puede visualizar como un “campo direccional” de flechitas que indican hacia dónde crece la solución.

3. **Curva solución:**

   - Resolver el PVI significa **encontrar la curva que sigue esas pendientes y que además pasa por el punto inicial $(t_0, x_0)$**.
   - Si no hubiera condición inicial, habría muchas curvas posibles.
   - La condición inicial selecciona **una única trayectoria**.

### Ejemplo intuitivo

Si tenemos:

$$
x' = -2x, \qquad x(0) = 3
$$

- La EDO sola ($x'=-2x$) describe una familia de soluciones: $x(t) = Ce^{-2t}$, con $C$ constante arbitraria.
- La condición inicial $x(0)=3$ obliga a que $C=3$.
- Así, la solución específica es $x(t) = 3e^{-2t}$.

👉 Geométricamente: de todas las curvas exponenciales decrecientes, elegimos la única que pasa por el punto $(0,3)$.

### Requerimientos de una solución de un PVI

Un **PVI** es:

$$
x'(t) = f(t, x), \qquad x(t_0) = x_0
$$

Para que una función $x(t)$ sea **solución**, debe cumplir:

1. **Diferenciabilidad:**

   - La función $x(t)$ debe ser **continua y diferenciable** en el intervalo donde se estudia.
   - Porque la ecuación diferencial involucra derivadas, y estas deben existir en todo el intervalo.

2. **Condición inicial:**

   - En el instante $t_0$, debe cumplirse exactamente

     $$
     x(t_0) = x_0
     $$

3. **Compatibilidad con la ecuación diferencial:**

   - Al sustituir $x(t)$ y su derivada $x'(t)$ en la ecuación

     $$
     x'(t) \stackrel{?}{=} f(t, x(t)),
     $$

     la igualdad debe cumplirse para todos los $t$ del intervalo.

4. **Continuidad global:**

   - La solución no puede tener “saltos” ni discontinuidades en el intervalo considerado.
   - Ejemplo: la ecuación $x' = \tfrac{1}{x}$.

     - La solución general es $x(t) = \pm \sqrt{2t + C}$.
     - En $x=0$, la función se hace discontinua → se rompen las condiciones de solución.
     - Por eso en realidad se obtienen **dos soluciones separadas** (una positiva y otra negativa), pero no una sola curva global.

## Teorema de Existencia y Unicidad

Este teorema es fundamental en el estudio de EDOs porque responde dos preguntas clave:

1. **¿Existe una solución que pase por el punto inicial?**
2. **Si existe, esa solución es única o puede haber varias?**

**Observación 2.2.3.** En los puntos $(x_0,y_0)$ donde no se cumplen las hipótesis del Teorema de existencia y Unicidad, pueden ocurrir varias cosas:

- puede que no exista solución pasando por $(x_0,y_0)$,
- puede que exista más de una solución pasando por $(x_0,y_0)$,
- puede que exista una única solución pasando por $(x_0,y_0)$.

### 1. Forma requerida

Escribe siempre el PVI en **forma normal**:

$$
y' = f(x,y),\qquad y(x_0)=y_0.
$$

Aquí $f(x,y)=x\sqrt{y}$.

### 2. Dónde valen las hipótesis (regiones)

Hipótesis del **Teorema de Existencia y Unicidad (Picard–Lindelöf)**:

- $f$ continua en una región $R$ que contenga $(x_0,y_0)$.
- $\partial f/\partial y$ continua en esa misma región (o $f$ es Lipschitz en $y$).

Para $f(x,y)=x\sqrt{y}$:

- **Continuidad de $f$:** definida y continua sólo para $y\ge 0$.
- **$\partial f/\partial y$:** $\displaystyle \frac{\partial f}{\partial y}=\frac{x}{2\sqrt{y}}$, **continua sólo para $y>0$** (es indefinida en $y=0$).

**Conclusión de regiones:**

- En $y>0$: $f$ y $\partial f/\partial y$ son continuas ⇒ **existe y es única** la solución local.
- En $y=0$: $f$ es continua pero $\partial f/\partial y$ no ⇒ **puede haber existencia sin unicidad**.
- En $y<0$: $f$ **no** está definida (en ℝ) ⇒ **no hay solución real**.

### 3. Casos según la condición inicial

#### (a) $y_0>0$  (punto encima del eje $x$)

- Por el teorema: **existe una única solución** en un intervalo alrededor de $x_0$.
- Mientras la trayectoria permanezca en $y>0$, la solución se **puede estirar** de forma única.
- El **límite de estiramiento** es cuando la curva toca la frontera $y=0$ (si la toca). Ahí **pierde unicidad**.

**Solución explícita** (separable en $y>0$):

$$
\frac{dy}{dx}=x\sqrt{y}
\;\Rightarrow\;
\int \frac{dy}{\sqrt{y}}=\int x\,dx
\;\Rightarrow\;
2\sqrt{y}=\frac{x^2}{2}+C.
$$

Con $y(x_0)=y_0$:

$$
\sqrt{y}=\frac{x^2-x_0^2}{4}+\sqrt{y_0}
\quad\Rightarrow\quad
\boxed{\,y(x)=\Big(\sqrt{y_0}+\tfrac{x^2-x_0^2}{4}\Big)^2\,}
\quad(\text{válida donde }y>0).
$$

**¿Dónde toca $y=0$?**
Resolver $\sqrt{y_0}+\frac{x^2-x_0^2}{4}=0$ ⇒

$$
x^2=x_0^2-4\sqrt{y_0}.
$$

- Si $x_0^2>4\sqrt{y_0}$ ⇒ hay puntos $x=\pm\sqrt{x_0^2-4\sqrt{y_0}}$ donde la solución **llega a $y=0$**.
  Hasta ese primer contacto hay **unicidad**; al tocar $y=0$, **no** hay unicidad hacia adelante.
- Si $x_0^2=4\sqrt{y_0}$ ⇒ la curva **tangencia** $y=0$ en un solo punto.
- Si $x_0^2<4\sqrt{y_0}$ ⇒ **nunca** toca $y=0$; la solución única se **estira globalmente** (para todo $x$).

#### (b) $y_0=0$  (punto sobre el eje $x$)

- El teorema **no garantiza** unicidad (falla $\partial f/\partial y$).
- **Existen varias soluciones** que pasan por $(x_0,0)$. Ejemplos (tomando $x_0=0$ para ilustrar):

  - $y(x)\equiv 0$ (solución constante).
  - $y(x)=\tfrac{1}{16}x^4$ (otra solución).
  - Más en general: soluciones **por tramos** que “se pegan” a $y=0$ un intervalo y luego despegan siguiendo la fórmula explícita anterior.
    ⇒ **No unicidad** en el eje $y=0$.

#### (c) $y_0<0$  (punto debajo del eje $x$)

- $\sqrt{y}$ no es real ⇒ **no hay solución real** que pase por $(x_0,y_0)$.

### 4. Lectura geométrica (qué “se ve” en el plano $x$-$y$)

- Eje horizontal: $x$. Eje vertical: $y$.
- Para $y>0$, las trayectorias son **“parábolas”** (curvas en forma de U) dadas por
  $\;y(x)=\big(\sqrt{y_0}+\frac{x^2-x_0^2}{4}\big)^2$, simétricas respecto a $x=0$.
- En el **eje $y=0$**:

  - Puntos iniciales **sobre** el eje: hay **múltiples** soluciones (p. ej. $0$ y $\tfrac{1}{16}x^4$).
  - Puntos iniciales **debajo** del eje: **no** hay solución.
- **Hasta dónde estirar:** desde $(x_0,y_0>0)$ se estira **mientras $y(x)>0$** (unicidad).
  El primer contacto con $y=0$ marca el **fin del intervalo de unicidad**.

### 5. Por qué importa (numéricamente)

- Si arrancas en $y_0>0$, los métodos (Euler/Runge–Kutta) funcionan bien **hasta** que la trayectoria se acerque a $y=0$; ahí puede aparecer **no unicidad** y bifurcaciones (la compu no “sabe” qué rama seguir).
- Si arrancas con $y_0=0$ o $y_0<0$, **sin análisis previo** el algoritmo puede “trabarse” o dar resultados inconsistentes.
- **Moral:** antes de simular, **verifica la región** donde $f$ y $\partial f/\partial y$ son continuas; determina **si hay unicidad** y **hasta dónde** puedes integrar de forma confiable.

## Análisis cualitativo de ecuaciones diferenciales

Cuando no tenemos (o no queremos calcular) la **solución exacta**, se puede estudiar el comportamiento de las soluciones usando **herramientas cualitativas**.

### 1. Análisis de signos de la función $f$

Sea un PVI en **forma normal**:

$$
y' = f(x,y).
$$

- La función $f(x,y)$ nos dice **la pendiente de la solución** en cada punto del plano.
- No necesito un punto inicial para dibujar el “campo direccional” de pendientes; puedo estudiar el **signo** de $f$.

#### Interpretación

- Si $f(x,y) > 0 \implies y'(x) > 0$:
  las curvas solución son **crecientes** en esa región.
- Si $f(x,y) < 0 \implies y'(x) < 0$:
  las curvas solución son **decrecientes** en esa región.
- Si $f(x,y) = 0$:
  hay una **solución constante** (recta horizontal), o una curva tangente a esa recta.

👉 Este análisis permite dibujar **hacia dónde se mueven las soluciones** sin resolver nada.

### 2. Análisis de concavidad

El siguiente paso es ver cómo **cambia la pendiente** (si sube cada vez más rápido o se aplana).
Para esto, se analiza la segunda derivada:

$$
y'' = \frac{d}{dx}(y') = \frac{d}{dx}\big(f(x,y)\big).
$$

Aplicando la regla de la cadena:

$$
y'' = \frac{\partial f}{\partial x}(x,y) + \frac{\partial f}{\partial y}(x,y)\cdot y'.
$$

#### Interpretación

- Si $y'' > 0$: la curva es **cóncava hacia arriba** (forma de “U”).
- Si $y'' < 0$: la curva es **cóncava hacia abajo** (forma de “∩”).
