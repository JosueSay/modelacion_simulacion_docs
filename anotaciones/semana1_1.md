# Introducción a la Modelación y Simulación

## ¿Qué es un modelo matemático?

Un **modelo matemático** es una descripción aproximada de un fenómeno del mundo real, expresada mediante símbolos y ecuaciones matemáticas. Su objetivo es representar, entender o predecir el comportamiento de dicho fenómeno.

## ¿Qué es la simulación?

La **simulación matemática y computacional** es un método para estudiar o predecir el comportamiento de un sistema a partir de su modelo matemático. Permite experimentar con el modelo sin necesidad de intervenir en el sistema real.

## Tipos de modelos

### Modelo directo

* Se basa en leyes conocidas (físicas, químicas, económicas, etc.).
* El modelo ya está formulado; el objetivo es **resolver** el sistema para predecir comportamientos o explicar situaciones pasadas.
* Ejemplo: Dada una ecuación que relaciona precios individuales con el precio de la canasta básica, calcular este último usando valores actuales.

### Modelo inverso

* No se parte de un modelo explícito, sino de **datos observados**.
* El objetivo es **encontrar el modelo** o ajustar parámetros que se adapten a los datos disponibles.
* Es más difícil que el modelo directo.
* Muy común en la práctica (por ejemplo, en aprendizaje automático).

## Ejemplos

### 1. Precio de la canasta básica

**Problema directo**:
Si hoy se conocen los precios de productos individuales
`x_hoy = (lb_papa, qt_frijol, p_tortilla) = (12.50, 180, 0.33)`
y se tiene un modelo lineal conocido, se puede calcular el precio de la canasta básica.

**Problema inverso**:
Dadas múltiples observaciones pasadas del precio de la canasta y los productos, se busca ajustar un modelo que relacione ambos.

### 2. Tomografía Axial Computarizada (CT)

* La máquina emite ondas en distintas direcciones que atraviesan el cuerpo.
* Se miden los tiempos de recorrido de esas ondas, que dependen de la densidad del material atravesado.

**Problema directo**:
Dada una función de densidad $u(x, y)$, calcular sus proyecciones (transformada de Radon).

**Problema inverso**:
A partir de las proyecciones, reconstruir la función $u$, es decir, obtener la imagen interna del cuerpo.

### 3. Deconvolución de imágenes

La imagen observada $f(t)$ es el resultado de la convolución de una imagen original $u(s)$ con un núcleo $K(t-s)$, más un ruido $n(t)$:

$$
f(t) = \int_{-\infty}^{\infty} K(t-s) u(s) ds + n(t)
$$

* Si $K$ es una función gaussiana, la imagen se desenfoca.
* **Problema directo**: Se conoce la imagen original y se genera la desenfocada.
* **Problema inverso**: A partir de la imagen desenfocada, se intenta recuperar la imagen original.

## Clasificación adicional de modelos

* **Continuos**: Se describen mediante ecuaciones diferenciales (ODEs, PDEs).
* **Discretos**: Se usan para fenómenos donde el tiempo o espacio está cuantizado.
* **Estocásticos (probabilísticos)**: Incluyen incertidumbre o ruido en el modelo.

En el curso se dará énfasis a modelos **continuos**, con aplicaciones a ecuaciones diferenciales, programación entera, algoritmos genéticos, entre otros.
