# Mínimos

## **Optimización 1-D y extensión a múltiples dimensiones**

En **optimización 1-D** (una sola variable) ya vimos métodos como **Golden Search**, **Interpolación parabólica** y **Newton**, donde buscamos el mínimo de una función

$$
f : [a,b] \subset \mathbb{R} \to \mathbb{R}
$$

En estos casos, la función tiene una sola entrada y una sola salida. El problema se reduce a encontrar el punto del intervalo $[a,b]$ donde $f$ alcanza su valor más pequeño.

Estos métodos funcionan bien en 1-D, pero **el mundo real rara vez es 1-D**. En Machine Learning, ingeniería, economía, física… casi siempre trabajamos con **muchas variables** al mismo tiempo. Por eso debemos extender las ideas a funciones:

$$
f: \mathbb{R}^n \to \mathbb{R}
$$

Aquí $x \in \mathbb{R}^n$ es un vector con varias variables:

$$
x = (x_1, x_2, \dots, x_n)^T
$$

y $f(x)$ es la **función objetivo** a minimizar.

### **Optimización no restringida vs restringida**

En programación lineal o problemas como el método simplex, transporte o asignaciones, trabajamos **dentro de una región factible** definida por restricciones: desigualdades, igualdades, cotas, etc. Esto se llama **optimización restringida**.

En cambio, en **optimización no restringida**, no imponemos límites: el conjunto factible $\Omega$ puede ser todo $\mathbb{R}^n$. El mínimo puede estar **en cualquier punto** del espacio. Muchos métodos basados en el gradiente funcionan en este escenario.

⚠️ Ojo: aunque sea “no restringido”, en la práctica $\Omega$ se **restringe al dominio de la función**. Si $f$ tiene saltos, divisiones por cero o regiones no definidas, no podemos evaluar ni optimizar ahí.

### **Variables continuas vs discretas**

En este curso tratamos $x \in \mathbb{R}^n$, es decir, **variables continuas**. Existe otro mundo llamado **optimización discreta** (por ejemplo, escoger combinaciones o rutas) que requiere técnicas distintas (búsqueda exhaustiva, branch & bound, heurísticas, etc.).
En general, la optimización continua es más “suave” y permite usar cálculo diferencial para encontrar mínimos y máximos.

## **Ejemplos importantes en optimización no restringida**

### 1. **Cociente de Rayleigh**

$$
\min_x \frac{x^\top A x}{x^\top x}, \quad A \text{ simétrica}
$$

* Si $A$ es la **matriz de covarianzas** de un conjunto de datos, este cociente mide la **varianza** de la nube de puntos en la dirección $x$.
* **Maximizarlo** → dirección de mayor varianza (**primer componente principal** en PCA).
* **Minimizarlo** → dirección de menor varianza.
* En PCA, las direcciones que buscamos corresponden a **autovectores** de $A$, y el cociente de Rayleigh está directamente ligado a los **autovalores**.

💡 **¿Por qué es útil?** En análisis de datos queremos reducir la dimensión quedándonos con las direcciones que más información (varianza) aportan. El cociente de Rayleigh es la herramienta matemática para hallarlas.

### 2. **Mínimos cuadrados con regularización de Tikhonov**

$$
\min_x \sum_{i=1}^n (x_i - y_i)^2 \;+\; \lambda \sum_{i=1}^{n-1} (x_{i+1}-x_i)^2
$$

* Primer término: mide **qué tan cerca están las predicciones $x_i$** de los datos reales $y_i$ (error de ajuste).
* Segundo término: penaliza cambios bruscos entre puntos consecutivos (fuerza la **suavidad**).
* $\lambda > 0$ controla el balance:

  * $\lambda$ pequeño → más ajuste (riesgo de overfitting).
  * $\lambda$ grande → más suavidad (puede subajustar).
* Este es un caso particular de **regularización**, que es clave en Machine Learning para evitar que el modelo aprenda demasiado el ruido de los datos.

💡 **¿Por qué es útil?** Porque casi todos los modelos reales deben generalizar bien, y eso se logra evitando oscilaciones excesivas en la predicción.

### 3. **Función de Rosenbrock**

$$
\min_x \sum_{i=1}^{n-1} \big[(x_{i+1}-x_i^2)^2 + (1-x_i)^2\big]
$$

* Paisaje con un valle muy estrecho y curvado.
* El gradiente se hace muy pequeño dentro del valle → los métodos de descenso por gradiente avanzan muy lento.
* Se usa como **función de prueba** para medir la eficiencia y robustez de algoritmos de optimización.

💡 **¿Por qué es útil?** Porque es un “campo de entrenamiento” para ver si un método puede manejar superficies complicadas donde la convergencia no es trivial.

## **El gradiente: dirección de crecimiento y decrecimiento máximo**

Para $f: \mathbb{R}^n \to \mathbb{R}$ diferenciable, el **gradiente** en un punto $p$ es:

$$
\nabla f(p) = \left( \frac{\partial f}{\partial x_1}(p), \dots, \frac{\partial f}{\partial x_n}(p) \right)^T
$$

* Indica **la dirección en la que $f$ crece más rápido**.
* Magnitud del gradiente → qué tan rápido crece en esa dirección.
* Dirección opuesta → **descenso más rápido** (base del **Gradient Descent**).

💡 **Interpretación geométrica:** el gradiente es **perpendicular** a las curvas de nivel de la función. Si te imaginas un mapa de montañas, el gradiente apunta cuesta arriba en la pendiente más empinada.

Perfecto, lo voy a rehacer en un texto limpio, completo y bien explicado, manteniendo **todo lo que pusiste** pero ordenándolo y extendiéndolo para que se entienda, sin omitir nada y reforzando con los “¿por qué?”, “¿para qué?” y “¿qué significa?”.

## **Gradiente, ortogonalidad y optimización**

En optimización, el **gradiente** es una de las herramientas más poderosas porque nos dice hacia dónde se mueve más rápidamente el valor de la función.
Una propiedad fundamental: **el gradiente en un punto siempre es ortogonal (perpendicular)** a las **curvas de nivel** de la función en ese punto.
Esto significa que si dibujamos las líneas donde la función toma un mismo valor (curvas de nivel), el gradiente apuntará siempre hacia afuera, en la dirección en la que el valor de la función crece más rápido.

### **Por qué importa la convexidad**

Cuando pasamos una función a un algoritmo de optimización, idealmente queremos que sea **convexa**.
¿Por qué? Porque una función convexa tiene una única forma de “U” (en 1D) o un único “valle” (en más dimensiones) y, por lo tanto, **un único mínimo global**.
Esto garantiza que si seguimos la dirección de descenso, inevitablemente llegaremos al mínimo buscado, sin riesgo de quedar atrapados en mínimos locales falsos.

En funciones más complicadas, con muchos mínimos, la situación cambia:

* Hay que diseñar estrategias, como **partir la búsqueda en intervalos** $[a,b]$, luego $[c,d]$, etc., para encontrar mínimos locales por zonas.
* En problemas con muchas irregularidades, es necesario un **estudio preliminar**: graficar, analizar, detectar posibles regiones de interés y decidir dónde aplicar el algoritmo.

### **Tipos de funciones y su complejidad**

Las funciones que encontramos en la práctica pueden tener:

* Regiones planas → el gradiente es muy pequeño y los algoritmos avanzan lento.
* Discontinuidades → no podemos calcular derivadas, se necesita otro enfoque.
* Múltiples mínimos → riesgo de quedarse atrapado en un mínimo local.
* Picos muy pronunciados → pueden desestabilizar el algoritmo.

Por eso, **antes de lanzar un algoritmo a ciegas**, hay que entender la función, graficarla, y anticipar dónde pueden estar los problemas.

### **Maximización como minimización**

En optimización, **casi todo se formula como un problema de minimización**.
Si queremos **maximizar** una función $g(x)$, basta con considerar $-g(x)$:

* El máximo de $g$ ocurre en el mismo punto que el mínimo de $-g$.
* Así podemos usar cualquier algoritmo de minimización para resolver problemas de maximización.

### **Gradiente, magnitud y dirección**

Cuando calculamos el gradiente:

* La **dirección** indica hacia dónde crece más rápido la función.
* La **magnitud** (longitud del vector gradiente) indica **cuán rápido crece**.

  * Magnitud grande → cambios abruptos.
  * Magnitud pequeña → cambios suaves o regiones planas.

Esto es clave en algoritmos: si la magnitud $\|\nabla f(x)\|$ es muy pequeña, podemos usarla como **criterio de paro** (hemos llegado a un punto donde el gradiente casi desaparece → posible mínimo).

### **Gradiente vs derivada (Jacobiano)**

En funciones $f: \mathbb{R}^n \to \mathbb{R}$:

* **Gradiente**: vector columna ($n \times 1$) con las derivadas parciales.

  $$
  \nabla f(x) =
  \begin{pmatrix}
  \frac{\partial f}{\partial x_1} \\
  \vdots \\
  \frac{\partial f}{\partial x_n}
  \end{pmatrix}
  $$
* **Derivada (Jacobiano)**: matriz fila ($1 \times n$) con las mismas entradas que el gradiente, pero escritas horizontalmente.

En funciones $f: \mathbb{R}^n \to \mathbb{R}^m$:

* El **Jacobiano** es una matriz $m \times n$, donde cada fila contiene las derivadas parciales de una de las salidas respecto a todas las variables de entrada.
* Si $m = 1$, el Jacobiano y el gradiente son lo mismo, salvo por la forma (fila vs columna).

**Regla de la cadena matricial**: para funciones compuestas $f(g(x))$,

$$
D(f \circ g)(p) = Df(g(p)) \cdot Dg(p)
$$

Se multiplican los Jacobianos como matrices.

### **Segunda derivada y Hessiano**

La segunda derivada en optimización multidimensional se representa con el **Hessiano**:

$$
H_f(p) =
\begin{pmatrix}
\frac{\partial^2 f}{\partial x_1^2} & \cdots & \frac{\partial^2 f}{\partial x_1 \partial x_n} \\
\vdots & \ddots & \vdots \\
\frac{\partial^2 f}{\partial x_n \partial x_1} & \cdots & \frac{\partial^2 f}{\partial x_n^2}
\end{pmatrix}
$$

Es una matriz $n \times n$ que describe la **curvatura** de la función alrededor de un punto.

### **Autovalores y curvatura**

El Hessiano es simétrico en funciones suaves, y por tanto tiene:

* Autovectores ortogonales → direcciones principales de curvatura.
* Autovalores reales → indican si la curvatura va hacia arriba o hacia abajo en cada dirección.

Interpretación:

* Todos los autovalores $> 0$ → curvatura hacia arriba → **mínimo local**.
* Todos $< 0$ → curvatura hacia abajo → **máximo local**.
* Mezcla de signos → **punto de silla**.

### **Mínimos locales y globales**

* **Mínimo local** $x^*$:

  $$
  f(x^*) \le f(x) \quad \text{para todo } x \text{ en un radio } r \text{ alrededor de } x^*
  $$

  Es el más bajo **solo en una vecindad**.

* **Mínimo global**:

  $$
  f(x^*) \le f(x) \quad \text{para todo } x \in \mathbb{R}^n
  $$

  Es el más bajo **en todo el dominio**.

Tipos:

* **Estricto**: la desigualdad es estricta ($<$) salvo en el propio punto.
* **No estricto**: hay varios puntos con el mismo valor mínimo.

Un mínimo puede ser **aislado** si en una vecindad pequeña no hay otros mínimos con el mismo valor.

### **Dificultades comunes en optimización**

* Múltiples mínimos locales → riesgo de quedarse atrapado.
* Regiones planas → gradiente muy pequeño, convergencia lenta.
* Funciones no diferenciables o discontinuas → gradiente no definido.
* Picos o cambios bruscos → pasos inestables.

Por eso, **siempre se recomienda estudiar y graficar** la función antes de aplicar un algoritmo, para no confiar ciegamente en el resultado numérico.

### **Descenso por gradiente**

El método más básico para encontrar mínimos locales:

1. Elegimos un punto inicial $x_0$.
2. Calculamos el gradiente $\nabla f(x_k)$.
3. Movemos el punto en dirección opuesta al gradiente:

   $$
   x_{k+1} = x_k - \alpha \, \nabla f(x_k)
   $$

   donde $\alpha$ es el **tamaño de paso**.
4. Repetimos hasta que $\|\nabla f(x_k)\|$ sea suficientemente pequeño (criterio de paro).

La elección de $\alpha$ es clave:

* Muy grande → riesgo de saltar el mínimo.
* Muy pequeña → convergencia lenta.
