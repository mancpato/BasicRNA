
# BasicRNA

Herramientas interactivas en el navegador para el análisis geométrico y paramétrico de modelos de clasificación binaria basados en redes neuronales.

Cada módulo es un archivo HTML autocontenido (Canvas 2D, JavaScript nativo `binary64`, sin dependencias ni compilación). Los parámetros se ajustan de forma manual mediante controles deslizantes para observar el impacto directo en la frontera de decisión ($\hat{y} = 0.5$) y en la tasa de error sin recurrir a rutinas de entrenamiento interactivo.

## Módulos

### 1. `1-Perceptron.html` — Perceptrón ($\mathbb{R}^2 \to [0, 1]$)

* **Arquitectura:** 2 entradas, salida con función sigmoide ($\sigma$).
* **Parámetros (3):** Pesos de entrada $\theta_0, \theta_1$ y sesgo $\theta_2$.
* **Ecuación de la frontera:** $\theta_0 x_1 + \theta_1 x_2 + \theta_2 = 0$.
* **Conjunto de datos:** 200 puntos linealmente separables (160 entrenamiento, 40 prueba) generados con distribuciones gaussianas centradas en $(-0.45, -0.38)$ y $(0.45, 0.40)$.
* **Objetivo:** Manipular la pendiente y el desplazamiento ortogonal de la recta en $\mathbb{R}^2$ hasta alcanzar cero errores de clasificación. Incluye pesos precalculados mediante descenso por gradiente: $\theta = [3.0041, 2.5568, 0.0366]$.

### 2. `2-EditParam.html` — Red 2→4→1 con capa homogénea

* **Arquitectura:** Capa de entrada ($d=2$), una capa oculta (4 neuronas) y una neurona de salida sigmoide.
* **Parámetros (17):**
  * $\theta_{0 \dots 3}$: Pesos $x_1 \to h_j$
  * $\theta_{4 \dots 7}$: Pesos $x_2 \to h_j$
  * $\theta_{8 \dots 11}$: Sesgos de la capa oculta $b_{h_j}$
  * $\theta_{12 \dots 15}$: Pesos capa oculta a salida $v_j$
  * $\theta_{16}$: Sesgo de salida $b_y$


* **Activación de capa oculta:** Selector global entre $\text{ReLU}$ y $\tanh$. Con $\tanh$ se muestra además el intervalo de activación que barre cada neurona oculta sobre las muestras.
* **Conjunto de datos:** Clases no linealmente separables (disco central contra anillo concéntrico exterior).
* **Objetivo:** Analizar la pérdida de intuición paramétrica individual en representaciones no lineales y contrastar la geometría de las fronteras resultantes (poligonal con $\text{ReLU}$ vs. suave con $\tanh$). Incluye configuraciones precalculadas con cero errores para ambas funciones.

### 3. `3-EditActivFun.html` — Red 2→4→1 con activación por neurona

* **Arquitectura:** Red 2→4→1 con salida sigmoide donde cada neurona oculta $h_j$ admite una función de activación independiente.
* **Catálogo de activaciones disponibles:**
  * Identidad (lineal)
  * Escalón unitario (Heaviside)
  * $\text{ReLU}$
  * Tangente hiperbólica ($\tanh$)
  * Gaussiana ($\exp(-z^2)$)
  * Seno ($\sin(z)$)


* **Conjuntos de datos:**
  1. *Banda:* Cero errores con 1 gaussiana o 2 $\text{ReLU}$ (mínimos hallados por búsqueda numérica; pesos precalculados obtenidos por ajuste numérico).
  2. *Círculos:* Problema concéntrico con cero errores con 2 gaussianas o 3 $\text{ReLU}$ (mínimos hallados por búsqueda numérica; pesos precalculados obtenidos por ajuste numérico).
  3. *Franjas periódicas:* Generado por el signo de $\sin(10 x_1)$, con 7 ceros en $(-1, 1)$, es decir 8 regiones en $[-1, 1]^2$. Resuelto exactamente con 1 neurona periódica: la solución está escrita a mano, $w_{x_1} = 10$, $b = 0$, $v = 4$ y el resto en cero, y reproduce la regla que generó las etiquetas.
     * *Cota para $\text{ReLU}$ y escalón:* con 4 neuronas $\text{ReLU}$, la salida previa a la sigmoide es lineal a trozos con a lo más 4 quiebres. Restringida a cualquier recta tiene a lo más 5 tramos y cruza el cero a lo más 4 veces, lo que da a lo más 5 regiones frente a las 8 de la regla. Con escalón la salida previa es constante a trozos con a lo más 4 saltos y el conteo es el mismo. Es una cota demostrada sobre la regla en el cuadrado, no un resultado de búsqueda.
     * *Gaussiana:* el conteo no aplica, porque cada neurona aporta dos cruces y 8 cruces bastarían en principio. Que 4 gaussianas no lleguen a cero errores es resultado de búsqueda numérica, no cota demostrada.


* **Rango del deslizador:** $[-12, 12]$ (paso 0.01) para permitir frecuencias angulares suficientes en la función seno.


## Especificaciones técnicas

* **Ancla estructural entre diagrama y vector de parámetros:**
  * El diagrama no se dibuja recorriendo la red sobre la marcha, sino a partir de un arreglo de descriptores, uno por parámetro, con su índice en $\theta$ y sus dos extremos en pantalla. El arreglo se verifica antes de crear ningún lienzo: si los índices no son una permutación completa de $0 \dots N-1$, la página no monta y muestra el error.
  * La selección con el ratón sigue la regla de que ante la duda no se selecciona nada. Hay candidata sólo si se cumplen tres condiciones: la arista más cercana cae dentro de la tolerancia (6 px), la segunda está al menos al doble de distancia, y además está a una separación mínima absoluta de la primera (2.5 px). Cerca de un nodo convergen hasta cinco aristas, y señalar la equivocada haría que el alumno creyera mover un parámetro mientras mueve otro.
  * El orden del vector es $\theta_{i \cdot N_{\text{oculta}} + j}$ (entrada $i$, neurona oculta $j$). Con el orden traspuesto, $j \cdot N_{\text{ent}} + i$, sólo coincidirían dos de las ocho aristas de entrada.
  * En `3-EditActivFun.html` el ancla tiene una segunda mitad: el nodo que se señala es la activación que cambia. Los cuatro nodos ocultos son círculos iguales, así que una permutación entre ellos no daría error visible. Por eso los nodos también tienen descriptores verificados, con índice de neurona y centro en pantalla, y el glifo lee el mismo arreglo de activaciones que usa la evaluación, sin copias.
* **Mapeo visual del vector de parámetros:**
* El grosor de cada conexión se calcula en función de $\vert{}\theta_i\vert{}$ normalizado contra el valor absoluto máximo del deslizador (tope fijo $[-6, 6]$ o $[-12, 12]$), aplicando una transformación de raíz cuadrada para conservar resolución en magnitudes pequeñas cercanas a cero. En `2-EditParam.html` y `3-EditActivFun.html` la opacidad sigue la misma escala; en `1-Perceptron.html` el color es sólido y sólo varía el grosor.
* Conexiones positivas en azul; negativas en rojo.

* **Frontera de decisión:** Evaluada en una retícula regular de $50 \times 50$ en $[-1, 1]^2$, interpolando linealmente los puntos de cruce donde la probabilidad posterior estimada $\hat{y}$ cruza el umbral $0.5$.
* **Muestreo de pesos iniciales:** Generador pseudoaleatorio determinista `splitmix32` con aritmética entera de 32 bits (`Math.imul`) sobre semillas fijas para reproducibilidad entre ejecuciones.