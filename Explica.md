
# BasicRNA

Herramientas interactivas en el navegador para el análisis geométrico y paramétrico de modelos de clasificación binaria basados en redes neuronales.

Cada módulo es un archivo HTML autocontenido (Canvas 2D, JavaScript nativo `binary64`, sin dependencias ni compilación). En los tres primeros módulos los parámetros se ajustan de forma manual mediante controles deslizantes, para observar el impacto directo en la frontera de decisión ($\hat{y} = 0.5$) y en la tasa de error sin recurrir a rutinas de entrenamiento. El cuarto módulo sí entrena: implementa descenso por gradiente estocástico con una muestra por iteración y expone un único hiperparámetro, la tasa de aprendizaje.

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

### 4. `4-Backprop.html` — Descenso por gradiente estocástico, un paso a la vez

* **Arquitectura y parámetros:** idénticos a los del módulo 2 (red 2→4→1, salida sigmoide, 17 parámetros en el mismo orden), y sobre el mismo conjunto de datos. Activación oculta conmutable entre $\text{ReLU}$ y $\tanh$; cada una conserva su propio vector de pesos, su contador de iteraciones, su historia de pérdida y su sorteador de puntos, de modo que el selector no reinicia ningún entrenamiento.

* **Función objetivo:** entropía cruzada binaria, es decir la log-verosimilitud negativa por punto,

  $$\mathcal{L}(\theta) = -\frac{1}{N}\sum_{n=1}^{N}\Big[y_n \log \hat{y}_n + (1-y_n)\log(1-\hat{y}_n)\Big],$$

  evaluada sobre las $N = 160$ muestras de entrenamiento tras cada actualización. El argumento del logaritmo se recorta a $[10^{-12}, 1-10^{-12}]$.

* **Paso hacia atrás.** Componer la entropía cruzada con la sigmoide de salida cancela las dos derivadas y deja la diferencia sin factores:

  $$\delta_y = \hat{y} - y, \qquad \delta_j = \delta_y\, v_j\, \varphi'(z_j),$$
  $$\frac{\partial \mathcal{L}_n}{\partial w_{ij}} = \delta_j x_i, \qquad
    \frac{\partial \mathcal{L}_n}{\partial b_{h_j}} = \delta_j, \qquad
    \frac{\partial \mathcal{L}_n}{\partial v_j} = \delta_y a_j, \qquad
    \frac{\partial \mathcal{L}_n}{\partial b_y} = \delta_y,$$

  con actualización $\Delta\theta = -\eta\, \partial \mathcal{L}_n / \partial \theta$ sobre una sola muestra, sin momento y sin lote. Derivadas de activación: $\varphi'(z) = [z > 0]$ para $\text{ReLU}$ —mismo convenio con que se evalúa, $\varphi(z) = z\,[z>0]$— y $\varphi'(z) = 1 - a^2$ para $\tanh$.

* **Inicialización:** Xavier uniforme, $w \sim U(-a, a)$ con $a = \sqrt{6/(n_{\text{ent}} + n_{\text{sal}})}$ por capa, es decir $a_1 = 1$ y $a_2 = \sqrt{6/5} \approx 1.0954$; sesgos en cero. Es constante del archivo, no perilla: se sortea con la semilla base, es la misma para las dos activaciones y el botón de reinicio devuelve exactamente ese vector. No se usa el muestreo uniforme sobre todo el recorrido del deslizador de los otros módulos: con pesos de magnitud 5 la sigmoide arranca saturada, el gradiente es casi nulo y la curva no baja en los primeros mil pasos.

* **Único hiperparámetro expuesto:** tasa de aprendizaje $\eta \in [0.01, 0.30]$, paso 0.01, valor inicial 0.05. Se congela al comenzar cada paso animado, de modo que lo dibujado coincida con lo aplicado.

* **Convergencia medida** (inicialización y semillas del archivo, $\eta = 0.05$): cero mal clasificados en ambas particiones en la iteración 1413 con $\text{ReLU}$ y 2401 con $\tanh$; en esas iteraciones $\max_i |\theta_i|$ vale 5.26 y 4.45 respectivamente. Continuando hasta $2\times10^4$ iteraciones el máximo ronda 7.8 con $\text{ReLU}$.

* **Controles de avance:** un paso animado en siete fases (500, 780, 680, 560, 480, 800 y 900 ms; 4.7 s en total) y tandas de 10 y 100 iteraciones con la animación desactivada, que ejecutan exactamente el mismo paso. Al terminar una tanda se apaga el resalte, para no presentar el último punto como representativo del conjunto. Con `prefers-reduced-motion: reduce` el paso se aplica sin animación y se muestra su lectura final.

* **Codificación visual del paso.** Se separan deliberadamente dos cantidades distintas definidas sobre la misma arista: la señal de error $\delta$, que depende del peso, y el ajuste $-\eta\,\delta_j a_i$, que además depende de la activación entrante.
  * $\delta$ se dibuja **en el nodo**, como anillo de grosor proporcional a $\sqrt{|\delta| / \max|\delta|}$ del paso.
  * El ajuste se dibuja **en la arista**, como pulso punteado de grosor proporcional a $\sqrt{|\Delta\theta_i| / \max_k |\Delta\theta_k|}$ del paso, con envolvente $\sin(\pi u)$.
  * El signo del ajuste usa dos tonos propios (violeta para incremento, ocre para decremento), porque azul y rojo ya codifican el signo del peso.
  * La propagación hacia adelante usa un marcador gris que recorre la arista con radio proporcional a la contribución $|\theta_i x_i|$ o $|\theta_i a_i|$ relativa a las de su grupo.
  * **Advertencia de escala:** el grosor del pulso está normalizado contra el mayor ajuste *del mismo paso*, no contra la magnitud del peso. Es la única escala no absoluta de todo el directorio, y responde a que los diecisiete ajustes de un paso con $\eta$ realista son del orden de $10^{-2}$ o menores. Queda declarado en la leyenda de la página.

* **Neuronas inactivas en el paso hacia atrás:** cuando $\varphi'(z_j) = 0$ —lo que con $\text{ReLU}$ ocurre siempre que $z_j \le 0$, y con $\tanh$ sólo por saturación numérica— la neurona no recibe señal y sus cinco aristas incidentes no cambian. La página atenúa esas aristas y rotula el nodo con $\varphi'=0$. Es el enlace con el escalón del módulo 3, cuya derivada es nula en todo punto donde existe.

* **Curva de la pérdida:** $\mathcal{L}$ frente al número de iteraciones, trazada desde la iteración cero y diezmada a un valor por columna de píxel (valores reales, no promedios de tramo). Incluye una marca en $\ln 2 \approx 0.693$, el valor de un clasificador que asigna probabilidad $0.5$ a toda muestra. Existe porque el contador de mal clasificados es entero y, actualizando de a un punto, brinca y ocasionalmente empeora: sin una cantidad continua al lado no hay forma de ver que el descenso progresa.

### 5. `5-Convolucion.html` — CNN 6×6 ya entrenada

* **Arquitectura:** imagen de $6 \times 6$, un filtro de $3 \times 3$ con sesgo y sin relleno, mapa de $4 \times 4$, activación $\text{ReLU}$, 16 nodos y salida sigmoide. Etiqueta $1$ = vertical.

* **Parámetros (27):**
  * $\theta_{0 \dots 8}$: pesos del filtro $w_{rs}$, por renglones
  * $\theta_{9}$: sesgo del filtro $b$
  * $\theta_{10 \dots 25}$: pesos $u_p$ de cada nodo a la salida
  * $\theta_{26}$: sesgo de salida $c$

  La celda $p$ del mapa está en la fila $i_p = \lfloor p/4 \rfloor$ (`p>>2`) y la columna $j_p = p \bmod 4$ (`p&3`).

* **Ecuaciones:**

  $$z_p = b + \sum_{r=0}^{2}\sum_{s=0}^{2} w_{rs}\, x_{i_p+r,\; j_p+s}, \qquad a_p = \max(z_p, 0), \qquad \hat{y} = \sigma\Big(c + \sum_{p=0}^{15} u_p\, a_p\Big).$$

* **Conjunto de datos:** 200 imágenes, un segmento de longitud 3 vertical u horizontal, con una sola intensidad por imagen en [0.75, 1], sobre fondo con ruido uniforme en [0, 0.25]. El trazo no toca las columnas 0 y 5: los horizontales empiezan en la columna 1 o 2. Sin ese margen esos trazos serían invisibles: medido con los pesos de la página 5, un vertical de 3 píxeles pintado en la columna 0 o en la 5 da la misma salida que el lienzo vacío, porque ninguna ventana lo tiene bajo su columna central y la ReLU apaga las celdas que lo ven. Estratificados: 80 verticales y 80 horizontales en las 160 de entrenamiento, 20 y 20 en las 40 de prueba, en orden mezclado dentro de cada partición.

* **Pesos incluidos:** La página trae dentro los pesos elegidos así: de los arranques con datos estratificados que llegaron a cero, el vector con mayor confianza mínima entre los que tienen todos sus parámetros con |θ| ≤ 6. La confianza mínima es la menor probabilidad que la red da a la clase correcta entre las 200 imágenes. Es el arranque 50128, con ReLU y η = 0.1, en la época 58: llegó a cero en la 8 y siguió 50 más. Confianza mínima 0.989, mayor |θ| 5.86, que es el de c, y cero errores en las 160 y en las 40. El vector completo lo da `elegir-pesos.js`, junto al reporte que se cita abajo.

* **Girar el filtro:** Un clic en el cuadrito del filtro lo gira y otro lo regresa; en el código es trasponer, que es lo que corresponde a trasponer la imagen. Sólo cambian los nueve números del filtro: b, los u_p y c se quedan iguales. La razón: el filtro es lo que la red busca, y cambiando el filtro la misma red busca otra cosa, sin reentrenar. La pregunta pasa a «¿Hay un trazo horizontal?», y los marcos de la galería y el contador se calculan contra ella. Medido con el arnés sobre el conjunto de la página: 29 errores en las 160 y 9 en las 40, que son exactamente las 38 horizontales de las filas 0 y 5, por la misma razón que un vertical en la columna 0 o 5: los datos no son simétricos, porque los verticales nunca tocan esas columnas y los horizontales sí tocan esas filas. Con trazos limpios en 1, las 16 horizontales de las filas 1 a 4 dicen «Sí», las 8 de las filas 0 y 5, las 24 verticales y el lienzo vacío dicen «No». La página verifica al montar que trasponer dos veces devuelve el filtro y que las mal clasificadas son exactamente esas 38.

* **Argumento de las tres columnas y las tres filas:** El perceptrón de la primera página no puede con estos datos: las tres columnas de un cuadrado 3×3 suman lo mismo que sus tres filas, así que ninguna función lineal de los píxeles separa las clases. La imposibilidad se atribuye a ese argumento aplicado a la regla que genera los datos (verificado numéricamente, 2.7e-15), no al contador: sobre la muestra, la programación lineal con los 36 píxeles empieza a fallar desde unas 76 imágenes, y eso mide la capacidad lineal, cuántas imágenes alcanza a separar una función lineal con esa cantidad de rasgos, no la estructura del problema.

### 6. `6-BackpropCNN.html` — Un filtro 3×3 que aprende

* **Arquitectura y parámetros:** los del módulo 5 (la misma red, 27 parámetros en el mismo orden), con activación conmutable entre $\text{ReLU}$ y lineal.

* **Paso hacia atrás:**

  $$\delta = \hat{y} - y, \qquad \delta_p = \delta\, u_p\, \varphi'(z_p),$$
  $$\frac{\partial \mathcal{L}}{\partial u_p} = \delta\, a_p, \qquad
    \frac{\partial \mathcal{L}}{\partial c} = \delta, \qquad
    \frac{\partial \mathcal{L}}{\partial b} = \sum_{p} \delta_p, \qquad
    \frac{\partial \mathcal{L}}{\partial w_{rs}} = \sum_{p} \delta_p\, x_{i_p+r,\; j_p+s},$$

  con actualización $\Delta\theta = -\eta\, \partial \mathcal{L} / \partial \theta$.

* **Entrenamiento:** SGD de a una imagen. Una época es una pasada por las 160 imágenes de entrenamiento, cada una una vez, en orden barajado de nuevo en cada época. Tasa $\eta \in [0.01, 0.20]$, inicial 0.05. Inicialización Xavier uniforme: $w$ en $\pm\sqrt{6/10}$, $u$ en $\pm\sqrt{6/17}$, $b = c = 0$. Sorteos con `splitmix32`. Activación $\text{ReLU}$ o lineal.

* **Escala del cambio:** una sola para los 27 parámetros, contra el mayor $|\Delta\theta|$ del paso, con raíz.

* **Al montar:** se verifican el ancla, los datos y el gradiente contra diferencias centradas (tolerancia $10^{-5}$), con un control que debe rechazar $b$ con el signo invertido.

* **Cifras medidas** (las midió Claude Code con el arnés, `reporte.txt`): La fuente es `../BasicRNA-trabajo/entrena-cnn/reporte.txt`, fuera del repositorio. Con ReLU y datos estratificados, 200 arranques por tasa, semilla de datos 20260928 y tope de 100 épocas. Llegar es tener cero errores en las 160 y en las 40 al final de una época. Un arranque apagado termina con todas las z_p ≤ 0 en las 200 imágenes: todos los nodos en cero y la misma salida para todas.

  ```
  tasa    llegan    época de llegada (mediana)    apagados
  0.01    75/200    36                             4
  0.02    83/200    21                             4
  0.05    79/200     8                             8
  0.1     68/200     5                            14
  0.2     44/200     3                            48
  ```

  Con lineal no llega ninguno, en ninguna tasa. Los apagados crecen con la tasa, y ésa es la razón para exponerla; enlaza con las neuronas muertas de la cuarta página. Entre el 25 % y el 51 % de los que llegan a cero vuelven a tener errores si se sigue entrenando 50 épocas más (con los estratificados, del 25 % al 46 %), y por eso la curva de la pérdida es necesaria. El éxito varía con el conjunto de datos: con ReLU y η = 0.05, en 21 conjuntos, va de 46 a 96 de 200 con sorteo libre, donde la semilla 20260928 da 96, la mejor de las 21 y no la típica (mediana 75); con los estratificados va de 60 a 92, y la 20260928 da 79, cerca de la mediana 76. La prueba casi nunca se separa del entrenamiento: con η = 0.05, 9 de 200 arranques estratificados tienen en alguna época cero errores en entrenamiento y alguno en prueba (8 de 200 con sorteo libre).

* **Arranque por omisión:** provisional, semilla 50013; la elección está abierta (`TODO.md` §4).

### 11. `11-LazoComoMatriz.html` — El lazo como matriz

La teoría de esta página —de un número a una matriz, el radio espectral $\rho$ y el mayor valor singular $\sigma_1$, la desigualdad fundamental y las cinco matrices preparadas, con sus cifras medidas— está en `doc/LazoComoMatriz.md`.

## Especificaciones técnicas

* **Ancla estructural entre diagrama y vector de parámetros:**
  * El diagrama no se dibuja recorriendo la red sobre la marcha, sino a partir de un arreglo de descriptores, uno por parámetro, con su índice en $\theta$ y sus dos extremos en pantalla. El arreglo se verifica antes de crear ningún lienzo: si los índices no son una permutación completa de $0 \dots N-1$, la página no monta y muestra el error.
  * La selección con el ratón sigue la regla de que ante la duda no se selecciona nada. Hay candidata sólo si se cumplen tres condiciones: la arista más cercana cae dentro de la tolerancia (6 px), la segunda está al menos al doble de distancia, y además está a una separación mínima absoluta de la primera (2.5 px). Cerca de un nodo convergen hasta cinco aristas, y señalar la equivocada haría que el alumno creyera mover un parámetro mientras mueve otro.
  * El orden del vector es $\theta_{i \cdot N_{\text{oculta}} + j}$ (entrada $i$, neurona oculta $j$). Con el orden traspuesto, $j \cdot N_{\text{ent}} + i$, sólo coincidirían dos de las ocho aristas de entrada.
  * En `3-EditActivFun.html` el ancla tiene una segunda mitad: el nodo que se señala es la activación que cambia. Los cuatro nodos ocultos son círculos iguales, así que una permutación entre ellos no daría error visible. Por eso los nodos también tienen descriptores verificados, con índice de neurona y centro en pantalla, y el glifo lee el mismo arreglo de activaciones que usa la evaluación, sin copias.
  * En `4-Backprop.html` no hay selección con el ratón, y el enunciado se invierte: la arista que se ilumina es el peso que cambia. El pulso del ajuste lee $\Delta\theta$ en el mismo índice que da el grosor, sin cálculo intermedio. Cada descriptor declara además de qué neurona oculta depende, y la verificación comprueba esa declaración contra el índice, de modo que atenuar las cinco aristas de una neurona con $\varphi'=0$ no dependa de un conteo hecho a mano.

* **Verificación del gradiente (`4-Backprop.html`):** antes de montar, los 17 gradientes analíticos se comparan con diferencias centradas de $\mathcal{L}_n$ ($h = 10^{-6}$, tolerancia relativa $10^{-6}$) sobre una muestra fija, en las dos activaciones. Con $\text{ReLU}$ se omite la comparación si alguna preactivación cae a menos de $10^{-4}$ del quiebre, donde la derivada no existe y la diferencia centrada promedia las pendientes laterales. Si la comprobación falla, la página no crea ningún lienzo y muestra el error: una animación de retropropagación que no retropropague la derivada de la función objetivo sería peor que su ausencia.

* **Mapeo visual del vector de parámetros:**
  * El grosor de cada conexión se calcula en función de $\vert{}\theta_i\vert{}$ normalizado contra un tope fijo —el valor absoluto máximo del deslizador, $[-6,6]$ o $[-12,12]$— aplicando una transformación de raíz cuadrada para conservar resolución en magnitudes pequeñas cercanas a cero. En `2-EditParam.html` y `3-EditActivFun.html` la opacidad sigue la misma escala; en `1-Perceptron.html` el color es sólido y sólo varía el grosor.
  * `4-Backprop.html` carece de deslizador de parámetros pero conserva el mismo tope, 6, para que un grosor dado signifique lo mismo en los cuatro módulos. Los valores que lo excedan se dibujan al grosor máximo; con $\eta = 0.05$ esto no ocurre antes de alcanzar cero errores.
  * Conexiones positivas en azul; negativas en rojo.

* **Frontera de decisión:** Evaluada en una retícula regular de $50 \times 50$ en $[-1, 1]^2$, interpolando linealmente los puntos de cruce donde la probabilidad posterior estimada $\hat{y}$ cruza el umbral $0.5$. Los puntos de cruce se ordenan angularmente respecto de su centroide, lo que supone un lazo estrellado respecto de ese centroide; con pesos no entrenados la curva puede autointersecarse, situación transitoria que no se corrige.

* **Muestreo pseudoaleatorio:** Generador determinista `splitmix32` con aritmética entera de 32 bits (`Math.imul`) sobre semillas fijas para reproducibilidad entre ejecuciones. En `4-Backprop.html` se usan dos flujos independientes: la semilla base para la inicialización de pesos y una semilla derivada para el sorteo de la muestra de cada iteración, de modo que la secuencia de puntos de una sesión de clase sea repetible.

* **Disposición:** `4-Backprop.html` está dimensionada para caber íntegra en una ventana de 940 px de alto, sin desplazamiento durante la clase. Esa restricción determina que ningún dato aparezca dos veces: las coordenadas de la muestra se leen junto a los nodos de entrada y $\hat{y}$ junto al de salida, de modo que los paneles de texto sólo contienen lo que el diagrama no dice.
