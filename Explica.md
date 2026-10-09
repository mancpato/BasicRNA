
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

Es la quinta y última de la serie recurrente. No entrena: los parámetros están
puestos y lo único que se recorre es el camino de regreso del gradiente.

#### Por qué existe

En `9-BackpropTiempo.html` la red tenía una sola neurona y la señal de gradiente retrocedía multiplicándose, en cada paso, por un número. Con dos neuronas ese número se vuelve una matriz de $2 \times 2$, y aparecen tres cosas nuevas:

1. **La dirección.** La señal ya no es un número sino una flecha en el plano. La matriz la estira en unas direcciones, la encoge en otras y puede girarla.

2. **Dos medidas en lugar de una.** Con una neurona bastaba ver si el factor era mayor o menor que 1. Con una matriz hay dos medidas distintas:
   * el **radio espectral** $\rho$ (el mayor tamaño de sus eigenvalores), que dice qué pasa *a la larga* cuando la misma matriz se repite muchas veces;
   * el **mayor valor singular** $\sigma_1$, que dice lo *más* que la matriz puede estirar una flecha en *un solo paso*.

   Siempre $\rho \le \sigma_1$. En algunas matrices son iguales; en otras son muy distintos, y ahí está la sorpresa.

3. **El factor cambia en cada momento.** La derivada de la activación, $\phi'$, depende del estado de cada neurona, así que la señal se multiplica por un producto de matrices *distintas*. Entonces los eigenvalores dejan de predecir lo que pasa, y sólo $\sigma_1$ sigue dando una garantía.

#### De un número a una matriz

| | Una neurona (`9-BackpropTiempo.html`) | Dos neuronas (esta página) |
|---|---|---|
| Memoria | número $h_t$ | vector $h_t$ (2 componentes) |
| Lazo | número $u$ | matriz $U$ ($2 \times 2$) |
| Pendiente de la activación | número $\phi'(z_t)$ | matriz diagonal $D_t$ |
| Señal de gradiente | número $\delta_t$ | vector $\delta_t$ |
| Factor de retroceso | $f_t = \phi'(z_t)\, u$ | $F_t = D_t\, U^T$ |
| $\rho$ y $\sigma_1$ | los dos valen $\lvert u \rvert$ | pueden ser muy distintos |
| Se apaga con seguridad si | $\lvert u \rvert < 1$ | $\sigma_1(U) < 1$ |

#### La red

En cada momento $t = 1, 2, \dots, T$:

$$z_t = W\,x_t + U\,h_{t-1} + b, \qquad h_t = \phi(z_t), \qquad \hat y = \operatorname{sig}(v\,h_T + c)$$

Con todos los arreglos escritos:

$$\begin{bmatrix} z_{t,1} \\ z_{t,2} \end{bmatrix} = \begin{bmatrix} w_1 \\ w_2 \end{bmatrix} x_t + \begin{bmatrix} u_{11} & u_{12} \\ u_{21} & u_{22} \end{bmatrix} \begin{bmatrix} h_{t-1,1} \\ h_{t-1,2} \end{bmatrix} + \begin{bmatrix} b_1 \\ b_2 \end{bmatrix}$$

$$\begin{bmatrix} h_{t,1} \\ h_{t,2} \end{bmatrix} = \begin{bmatrix} \phi(z_{t,1}) \\ \phi(z_{t,2}) \end{bmatrix}, \qquad \hat y = \operatorname{sig}\left( \begin{bmatrix} v_1 & v_2 \end{bmatrix} \begin{bmatrix} h_{T,1} \\ h_{T,2} \end{bmatrix} + c \right)$$

Qué es cada símbolo:

* $x_t$: la entrada en el momento $t$ (un número; en esta página, 0 o 1).
* $W$: columna de $2 \times 1$ con los pesos de la entrada a cada neurona.
* $U$: matriz de $2 \times 2$ con los pesos de las cuatro líneas de regreso (arquitectura de Elman).
* $b$: columna de $2 \times 1$ con los sesgos.
* $h_0 = \begin{bmatrix} 0 \\ 0 \end{bmatrix}$: la memoria antes de empezar.
* $\phi$: la activación de las neuronas de memoria ($\tanh$, o la identidad en modo lineal).
* $v$: renglón de $1 \times 2$ con los pesos hacia la neurona de salida; $c$: su sesgo.
* $\operatorname{sig}$: la sigmoide (logística) de la neurona de salida. Se escribe $\operatorname{sig}$ y no $\sigma$ porque en esta página $\sigma_1$ y $\sigma_2$ son los valores singulares.

**Convención de índices.** $u_{ij}$ es el peso de la línea que va **de la neurona $j$ a la neurona $i$**. Se ve al escribir el primer renglón:

$$z_{t,1} = w_1\, x_t + u_{11}\, h_{t-1,1} + u_{12}\, h_{t-1,2} + b_1$$

$u_{12}$ multiplica la memoria de la neurona 2, y el resultado llega a la neurona 1.

**Momentos y pasos hacia atrás.** $T = 40$, y con 40 momentos hay 39 pasos hacia atrás: el contador $k$ de pasos hacia atrás va de 0 a 39, y $t = T - k$ es el momento por el que va el retroceso.

**El empuje.** $W$ y $b$ se escalan juntos con un solo deslizador, el **empuje** $a$, que va de 0 a 2 y empieza en 0.2:

$$W = a \begin{bmatrix} 1 \\ 0.5 \end{bmatrix}, \qquad b = a \begin{bmatrix} 0.3 \\ -0.2 \end{bmatrix}$$

Con $a = 0$ nada empuja a las neuronas, la memoria se queda en cero y $\tanh$ da lo mismo que lineal. En modo lineal el deslizador cambia la memoria pero no la señal de gradiente: en lineal, el retroceso no depende de los datos. El valor inicial es 0.2 y no 1 porque con $a = 1$ las cinco matrices quedan por debajo de $10^{-2}$ hacia el paso 20 —la joroba de la matriz 4, por ejemplo, ya no pasa de 1— y se borra lo que la página quiere mostrar.

Las dos neuronas reciben entrada y sesgo distintos a propósito; la razón está en la matriz 5, abajo. La entrada son 40 bits sorteados una vez con `splitmix32` y la semilla 20261007.

#### La señal de gradiente

Ya no hablamos sólo de "error": seguimos la **señal de gradiente**, el vector

$$\delta_t = \begin{bmatrix} \dfrac{\partial L}{\partial z_{t,1}} \\[2ex] \dfrac{\partial L}{\partial z_{t,2}} \end{bmatrix} = \begin{bmatrix} \delta_{t,1} \\ \delta_{t,2} \end{bmatrix}$$

donde $L$ es la pérdida. Cada componente dice cuánto cambiaría la pérdida si se moviera un poco la entrada de esa neurona en el momento $t$.

**Cómo retrocede.** La memoria $h_t$ sólo influye en la pérdida a través de $z_{t+1}$. La regla de la cadena se aplica en dos pasos.

*Paso 1, por las líneas de regreso.* La componente $h_{t,j}$ entra a la neurona $i$ con peso $u_{ij}$, así que

$$\begin{bmatrix} \dfrac{\partial L}{\partial h_{t,1}} \\[2ex] \dfrac{\partial L}{\partial h_{t,2}} \end{bmatrix} = \begin{bmatrix} u_{11}\,\delta_{t+1,1} + u_{21}\,\delta_{t+1,2} \\ u_{12}\,\delta_{t+1,1} + u_{22}\,\delta_{t+1,2} \end{bmatrix} = \underbrace{\begin{bmatrix} u_{11} & u_{21} \\ u_{12} & u_{22} \end{bmatrix}}_{U^T} \begin{bmatrix} \delta_{t+1,1} \\ \delta_{t+1,2} \end{bmatrix}$$

*Paso 2, por la activación.* Como $h_{t,j} = \phi(z_{t,j})$, cada componente se multiplica por la pendiente de su propia neurona:

$$\begin{bmatrix} \delta_{t,1} \\ \delta_{t,2} \end{bmatrix} = \underbrace{\begin{bmatrix} \phi'(z_{t,1}) & 0 \\ 0 & \phi'(z_{t,2}) \end{bmatrix}}_{D_t} \begin{bmatrix} \dfrac{\partial L}{\partial h_{t,1}} \\[2ex] \dfrac{\partial L}{\partial h_{t,2}} \end{bmatrix}$$

Juntando los dos pasos:

$$\delta_t = F_t\,\delta_{t+1}, \qquad F_t = D_t\,U^T$$

$$\begin{bmatrix} \delta_{t,1} \\ \delta_{t,2} \end{bmatrix} = \underbrace{\begin{bmatrix} \phi'(z_{t,1}) & 0 \\ 0 & \phi'(z_{t,2}) \end{bmatrix}}_{D_t} \underbrace{\begin{bmatrix} u_{11} & u_{21} \\ u_{12} & u_{22} \end{bmatrix}}_{U^T} \begin{bmatrix} \delta_{t+1,1} \\ \delta_{t+1,2} \end{bmatrix}$$

**La red avanza con $U$; el gradiente retrocede con $U^T$.** Es la misma línea recorrida al revés. Ejemplo con $u_{12}$:

* Hacia adelante, lleva la memoria de la neurona 2 a la neurona 1: aparece en $z_{t,1}$ multiplicando a $h_{t-1,2}$.
* Hacia atrás, lleva la señal de la neurona 1 a la neurona 2: aparece en $\delta_{t,2}$ multiplicando a $\delta_{t+1,1}$. Por eso en $U^T$ ocupa el renglón 2, columna 1.

**Qué hace $D_t$.** Con $\phi = \tanh$, la pendiente se lee directo de la memoria:

$$\phi'(z) = 1 - \tanh^2(z) = 1 - h^2, \qquad D_t = \begin{bmatrix} 1 - h_{t,1}^2 & 0 \\ 0 & 1 - h_{t,2}^2 \end{bmatrix}$$

Cada número de la diagonal está entre 0 y 1: vale 1 sólo si la neurona está en cero, y se acerca a 0 cuando la neurona se satura ($h$ cerca de $\pm 1$). $D_t$ nunca estira; sólo encoge cada componente según qué tan saturada esté su neurona. En modo lineal, $\phi(z) = z$, $\phi' = 1$ y $D_t$ es la identidad.

#### Dónde empieza la señal

En el último momento, con la sigmoide de salida y la entropía cruzada como pérdida,

$$\begin{bmatrix} \delta_{T,1} \\ \delta_{T,2} \end{bmatrix} = \begin{bmatrix} \phi'(z_{T,1}) & 0 \\ 0 & \phi'(z_{T,2}) \end{bmatrix} \begin{bmatrix} v_1 \\ v_2 \end{bmatrix} (\hat y - y)$$

donde $y$ es la respuesta correcta. En una red que se entrena, la dirección de $\delta_T$ la deciden $v$ y $D_T$. En esta página no se entrena: $\delta_T$ **lo elige el alumno** (arrastrando la flecha) o lo pone la matriz preparada, porque lo que se estudia es qué hace el lazo con cada dirección. Todas las flechas iniciales de las matrices preparadas miden 1. Por eso $v$, $c$ e $y$ no intervienen en los cálculos de la página: sólo explican de dónde vendría $\delta_T$.

#### Medir cuánto estira una matriz

**Largo de una flecha.** $\Vert \delta \Vert = \sqrt{\delta_1^2 + \delta_2^2}$.

**Lo más que estira una matriz.** Para una matriz $A$ de $2 \times 2$,

$$\Vert A \Vert_2 = \text{el mayor largo de } A\,\delta \text{ entre todas las flechas } \delta \text{ de largo 1.}$$

Ese número es el **mayor valor singular** $\sigma_1(A)$. Geométricamente: $A$ convierte el círculo de radio 1 en una elipse; el semieje mayor mide $\sigma_1$ y el semieje menor mide $\sigma_2$, el **menor valor singular**. Si $\sigma_2 = 0$, la elipse se aplasta en un segmento.

Tres hechos que usa la página:

* Para una matriz diagonal, $\Vert D \Vert_2$ es el mayor de los números de su diagonal (en valor absoluto). Con $\tanh$, $\Vert D_t \Vert_2 \le 1$.
* Transponer no cambia los valores singulares: $\Vert U^T \Vert_2 = \Vert U \Vert_2 = \sigma_1(U)$. Por eso el techo del retroceso se calcula con $U$ aunque el gradiente use $U^T$.
* $U$ y $U^T$ tienen los mismos eigenvalores, y por tanto el mismo $\rho$.

**Radio espectral.** $\rho(U)$ es el mayor tamaño $\lvert \lambda \rvert$ entre los eigenvalores $\lambda$ de $U$. Si la *misma* matriz se aplica muchas veces, el largo de la señal termina cambiando, en promedio por paso, por el factor $\rho$.

**Siempre $\rho \le \sigma_1$.** Si $q$ es un eigenvector, $U^T q = \lambda\, q$, y entonces $\Vert U^T q \Vert = \lvert \lambda \rvert\, \Vert q \Vert$. Pero ninguna flecha se estira más que $\sigma_1$, así que $\lvert \lambda \rvert \le \sigma_1$. (Para eigenvalores complejos el argumento es el mismo, con flechas complejas.)

#### La desigualdad fundamental

$$\Vert \delta_t \Vert = \Vert D_t\,U^T \delta_{t+1} \Vert \;\le\; \Vert D_t \Vert_2 \; \Vert U^T \delta_{t+1} \Vert \;\le\; \Vert D_t \Vert_2 \; \Vert U^T \Vert_2 \; \Vert \delta_{t+1} \Vert$$

Con $\tanh$, $\Vert D_t \Vert_2 \le 1$, y además $\Vert U^T \Vert_2 = \sigma_1(U)$:

$$\Vert \delta_t \Vert \le \sigma_1(U)\,\Vert \delta_{t+1} \Vert$$

Al retroceder $k$ pasos se acumula:

$$\Vert \delta_{t-k} \Vert \le \sigma_1(U)^k\,\Vert \delta_t \Vert$$

**Es un techo, no un piso.**

* Si $\sigma_1(U) < 1$, la señal se apaga **con seguridad**, sin importar los datos ni qué tan saturadas estén las neuronas.
* Si $\sigma_1(U) > 1$, la señal **puede** crecer, pero no está obligada: el techo sólo dice hasta dónde podría llegar.

**Por qué ésta es la fundamental.** Cuando el factor cambia en cada paso, los eigenvalores de cada factor no dicen qué hace el producto. Ejemplo:

$$N_1 = \begin{bmatrix} 0 & 2 \\ 0 & 0 \end{bmatrix}, \qquad N_2 = \begin{bmatrix} 0 & 0 \\ 2 & 0 \end{bmatrix}, \qquad N_1 N_2 = \begin{bmatrix} 4 & 0 \\ 0 & 0 \end{bmatrix}$$

$N_1$ y $N_2$ tienen sus dos eigenvalores iguales a 0, y su producto tiene un eigenvalor igual a 4. En cambio, el estiramiento máximo sí se porta bien con los productos, $\Vert A B \Vert_2 \le \Vert A \Vert_2\, \Vert B \Vert_2$, y eso es lo único que usa la desigualdad. Por eso sigue valiendo aunque $D_t$ cambie en cada momento.

#### Las matrices preparadas

Las tres primeras son **normales** ($U U^T = U^T U$): para ellas $\sigma_1 = \rho$ y las dos medidas cuentan la misma historia. Las dos últimas no son normales, y ahí aparece la brecha. Además, las matrices 1, 3 y 4 tienen el mismo $\rho = 0.9$, para poder compararlas.

| Matriz | Eigenvalores | $\rho$ | $\sigma_1$ | $\sigma_2$ | ¿Normal? | $\delta_T$ inicial |
|---|---|---|---|---|---|---|
| 1. Diagonal | 0.9 y 0.5 | 0.9 | 0.9 | 0.5 | sí | $(0.707,\ 0.707)$ |
| 2. Simétrica | 1.2 y 0.4 | 1.2 | 1.2 | 0.4 | sí | $(1,\ 0)$ |
| 3. Giro | $0.9\,e^{\pm i\,30^\circ}$ | 0.9 | 0.9 | 0.9 | sí | $(1,\ 0)$ |
| 4. Joroba | 0.9 (doble) | 0.9 | ≈ 1.530 | ≈ 0.530 | no | $(1,\ 0)$ |
| 5. Nilpotente | 0 (doble) | 0 | 4 | 0 | no | $(0.707,\ 0.707)$ |

($0.707 \approx \sqrt{2}/2$, para que la flecha mida 1.)

Las cifras en modo lineal que se citan abajo salen de la fórmula cerrada de cada matriz y se confirmaron iterando la matriz paso a paso, con una diferencia peor de $9 \times 10^{-16}$; las de modo $\tanh$ las midió Claude Code con un arnés en Node, `binary64`, con el empuje inicial $a = 0.2$. La página las vuelve a verificar al montar, y no monta si alguna falla.

##### 1. Matriz diagonal

$$U = U^T = \begin{bmatrix} 0.9 & 0 \\ 0 & 0.5 \end{bmatrix}$$

* **Modo lineal.** Las dos componentes evolucionan sin mezclarse: la primera se multiplica por 0.9 y la segunda por 0.5 en cada paso.
$$\delta_{T-k} = \begin{bmatrix} 0.707 \cdot 0.9^k \\ 0.707 \cdot 0.5^k \end{bmatrix}$$
A los 5 pasos, $\delta_{T-5} \approx (0.418,\ 0.022)$. La flecha no gira, pero queda casi sobre el eje de la neurona 1, porque esa componente sobrevive más.
* **Modo tanh.** $D_t$ y $U^T$ son diagonales, así que las neuronas siguen sin mezclarse; cada componente sólo se encoge un poco más.
* **Plano.** La elipse tiene sus ejes sobre los ejes del plano (semiejes 0.9 y 0.5). Esos dos ejes son las direcciones que no giran.

##### 2. Matriz simétrica

$$U = U^T = \begin{bmatrix} 0.8 & 0.4 \\ 0.4 & 0.8 \end{bmatrix}$$

Eigenvectores: $(1,1)$ con eigenvalor 1.2, y $(1,-1)$ con eigenvalor 0.4.

* **Modo lineal.** La flecha inicial se descompone en esas dos direcciones:
$$\begin{bmatrix} 1 \\ 0 \end{bmatrix} = \frac{1}{2} \begin{bmatrix} 1 \\ 1 \end{bmatrix} + \frac{1}{2} \begin{bmatrix} 1 \\ -1 \end{bmatrix} \quad\Longrightarrow\quad \delta_{T-k} = \frac{1}{2}\,(1.2)^k \begin{bmatrix} 1 \\ 1 \end{bmatrix} + \frac{1}{2}\,(0.4)^k \begin{bmatrix} 1 \\ -1 \end{bmatrix}$$
Primeros pasos:
$$\begin{bmatrix} 1 \\ 0 \end{bmatrix} \to \begin{bmatrix} 0.8 \\ 0.4 \end{bmatrix} \to \begin{bmatrix} 0.8 \\ 0.64 \end{bmatrix} \to \cdots$$
La parte de 0.4 desaparece pronto y la flecha queda sobre $(1,1)$, creciendo: largo ≈ 4.38 a los 10 pasos y 866 en el paso 39, el último.
* **Modo tanh.** Aquí la historia se voltea. El eigenvalor 1.2 empuja la memoria hacia la saturación. Sin entrada ni sesgo, una vez que la memoria sale de cero se asienta cerca de $h \approx (0.66,\ 0.66)$, donde
$$D_t \approx \begin{bmatrix} 0.57 & 0 \\ 0 & 0.57 \end{bmatrix}, \qquad F_t \approx 0.57\, U^T$$
y los eigenvalores de $F_t$ quedan en $0.57 \times 1.2 \approx 0.68$ y $0.57 \times 0.4 \approx 0.23$: la señal se apaga. Con la entrada de la página la caída es más fuerte todavía: desde $\delta_T = (1, 0)$ la señal cae a 0.34 en el primer paso y a $7 \times 10^{-7}$ en el paso 20, contra los 866 del modo lineal.

  **Lección:** una memoria que se acomoda en un lugar estable (si la empujas un poco, regresa) no deja pasar el gradiente lejos. La misma estabilidad que evita que la memoria se descontrole hace que la señal de regreso muera.
* **Plano.** Elipse con ejes en las diagonales $(1,1)$ y $(1,-1)$, semiejes 1.2 y 0.4.

##### 3. Matriz de giro

$$U = 0.9 \begin{bmatrix} \cos 30^\circ & -\sin 30^\circ \\ \sin 30^\circ & \cos 30^\circ \end{bmatrix} \approx \begin{bmatrix} 0.779 & -0.450 \\ 0.450 & 0.779 \end{bmatrix}, \qquad U^T \approx \begin{bmatrix} 0.779 & 0.450 \\ -0.450 & 0.779 \end{bmatrix}$$

Sus eigenvalores son complejos, $0.9\,e^{\pm i\,30^\circ}$: no hay ninguna dirección que se quede sin girar.

* **Los dos giros.** $U$ gira $30^\circ$ en sentido contrario a las manecillas del reloj; $U^T$ gira $30^\circ$ **en el sentido de las manecillas**. La memoria y la señal de gradiente giran en sentidos opuestos: es la transpuesta, vista.
* **Modo lineal.**
$$\begin{bmatrix} 1 \\ 0 \end{bmatrix} \to \begin{bmatrix} 0.779 \\ -0.450 \end{bmatrix} \to \cdots$$
La flecha da una vuelta completa cada 12 pasos y se encoge por 0.9 en cada uno: al terminar la primera vuelta mide $0.9^{12} \approx 0.28$.
* **Modo tanh.** $D_t$ deforma el círculo en elipse; el giro deja de ser parejo y la flecha se encoge más rápido.
* **Plano.** $\sigma_1 = \sigma_2 = 0.9$, así que la "elipse" es un círculo de radio 0.9: la matriz no prefiere ninguna dirección.

##### 4. Matriz no normal: la joroba

$$U = \begin{bmatrix} 0.9 & 1 \\ 0 & 0.9 \end{bmatrix}, \qquad U^T = \begin{bmatrix} 0.9 & 0 \\ 1 & 0.9 \end{bmatrix}$$

Su único eigenvalor es 0.9 (doble), igual que el $\rho$ de las matrices 1 y 3. Pero no es normal:

$$U U^T = \begin{bmatrix} 1.81 & 0.9 \\ 0.9 & 0.81 \end{bmatrix} \ne \begin{bmatrix} 0.81 & 0.9 \\ 0.9 & 1.81 \end{bmatrix} = U^T U$$

y su mayor valor singular es $\sigma_1 \approx 1.530$.

* **Qué hace $U^T$.** Escrita por componentes:
$$\delta_{t,1} = 0.9\,\delta_{t+1,1}, \qquad \delta_{t,2} = 1 \cdot \delta_{t+1,1} + 0.9\,\delta_{t+1,2}$$
La componente 1 se apaga despacio (0.9 por paso) y, en cada paso, le pasa su señal completa a la componente 2, que la va acumulando. Es la línea $u_{12}$ recorrida al revés: hacia adelante, la neurona 2 alimenta a la neurona 1; hacia atrás, la señal de la neurona 1 alimenta a la neurona 2.
* **Primeros pasos en modo lineal**, desde $\delta_T = (1, 0)$:
$$\begin{bmatrix} 1 \\ 0 \end{bmatrix} \to \begin{bmatrix} 0.9 \\ 1 \end{bmatrix} \to \begin{bmatrix} 0.81 \\ 1.8 \end{bmatrix} \to \begin{bmatrix} 0.729 \\ 2.43 \end{bmatrix} \to \cdots$$
* **Fórmula general.** Como $U^T = 0.9\,I + N$ con $N = \begin{bmatrix} 0 & 0 \\ 1 & 0 \end{bmatrix}$, y $N N = 0$, del binomio sólo sobreviven dos términos:
$$(U^T)^k = 0.9^k\, I + k\,0.9^{k-1} N = \begin{bmatrix} 0.9^k & 0 \\ k\,0.9^{k-1} & 0.9^k \end{bmatrix}, \qquad \delta_{T-k} = \begin{bmatrix} 0.9^k \\ k\,0.9^{k-1} \end{bmatrix}$$
* **Largo de la señal:**

| Pasos hacia atrás $k$ | 0 | 1 | 2 | 3 | 5 | 9 | 20 | 35 | 39 |
|---|---|---|---|---|---|---|---|---|---|
| $\Vert \delta_{T-k} \Vert$ | 1 | 1.35 | 1.97 | 2.54 | 3.33 | 3.89 | 2.70 | 0.97 | 0.71 |

  La señal crece hasta casi 4 veces su tamaño hacia el paso 9, y no regresa a su tamaño original sino hasta el paso 35.
* **La comparación clave.** Las matrices 1, 3 y 4 tienen $\rho = 0.9$. A los 9 pasos, desde $(1, 0)$, la matriz de giro deja la señal en $0.9^9 \approx 0.39$ y ésta la deja en 3.89: diez veces más. "Eventualmente se apaga" y "nunca crece mucho" son afirmaciones distintas.
* **El techo es holgado.** A los 9 pasos permite $\sigma_1^9 \approx 46$, y la señal llega a 3.89.
* **La dirección importa.** El único eigenvector de $U^T$ es $(0, 1)$. Si el alumno arrastra $\delta_T$ a $(0, 1)$, la señal sólo se encoge por 0.9 en cada paso y no hay joroba. Desde cualquier otra dirección, la flecha termina alineada con $(0, 1)$.
* **Modo tanh.** La joroba se achata: con el empuje inicial sube sólo hasta 1.26, contra 3.89 en lineal. Cuánto se achate depende de qué tan saturadas estén las neuronas, y por eso el empuje es un deslizador y no una constante.
* **Plano.** Elipse alargada, de semiejes ≈ 1.530 y ≈ 0.530, aunque el eigenvalor sea 0.9.

##### 5. Matriz nilpotente: explosión y desaparición

$$U = \begin{bmatrix} 2 & -2 \\ 2 & -2 \end{bmatrix}, \qquad U^T = \begin{bmatrix} 2 & 2 \\ -2 & -2 \end{bmatrix}$$

$\rho = 0$ (sus dos eigenvalores son 0), pero $\sigma_1 = 4$. Además, la matriz multiplicada por sí misma se anula:

$$U^T U^T = \begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix}$$

* **Por qué.** $U^T$ se escribe como columna por renglón:
$$U^T = \begin{bmatrix} 2 \\ -2 \end{bmatrix} \begin{bmatrix} 1 & 1 \end{bmatrix}, \qquad U^T \begin{bmatrix} \delta_1 \\ \delta_2 \end{bmatrix} = (\delta_1 + \delta_2) \begin{bmatrix} 2 \\ -2 \end{bmatrix}$$
Primero suma las dos componentes y después reparte esa suma en la dirección $(2, -2)$. Pero esa dirección suma cero: $2 + (-2) = 0$. En el siguiente paso no queda nada. Dicho de otro modo: $U^T$ manda todo a la línea de $(1,-1)$, y esa misma línea es la que manda a cero.
* **Modo lineal**, desde $\delta_T = (0.707,\ 0.707)$:
$$\begin{bmatrix} 0.707 \\ 0.707 \end{bmatrix} \to \begin{bmatrix} 2.828 \\ -2.828 \end{bmatrix} \to \begin{bmatrix} 0 \\ 0 \end{bmatrix}$$
La señal se multiplica por 4 en un paso y desaparece en el siguiente. El cero es exacto en punto flotante, porque se suman dos números opuestos idénticos; ahí se detiene el retroceso.
* **Modo tanh.** Entre los dos pasos se mete $D_t$, con números $d_1$ y $d_2$ en su diagonal:
$$\begin{bmatrix} d_1 & 0 \\ 0 & d_2 \end{bmatrix} \begin{bmatrix} 2 \\ -2 \end{bmatrix} = \begin{bmatrix} 2 d_1 \\ -2 d_2 \end{bmatrix}, \qquad \text{que suma } 2(d_1 - d_2)$$
Sólo se anula si las dos neuronas tienen la misma pendiente. Con $\tanh$ la señal ya no desaparece: la frena la diferencia entre las dos pendientes. Con el empuje inicial, la señal pasa de 3.53 a 0.91 en el segundo paso, en vez de a cero.
* **Plano.** La elipse se aplasta en un segmento de semilargo 4 en la dirección $(1,-1)$; en el paso siguiente, en un punto.

**Por qué las dos neuronas reciben entrada y sesgo distintos.** Como los dos renglones de $U$ son iguales, $z_{t,1} - z_{t,2} = (w_1 - w_2)\,x_t + (b_1 - b_2)$ no depende de la memoria. Si las dos neuronas recibieran la misma entrada y el mismo sesgo, tendrían siempre la misma pendiente y la matriz seguiría siendo nilpotente aun con $\tanh$. Con los valores de la página esa diferencia vale $0.5\,a$ o $a$, nunca cero cuando $a > 0$.

#### Lo que el alumno se lleva

1. La red avanza con $U$; la señal de gradiente retrocede con $U^T$: las mismas líneas, al revés.
2. $\rho$ dice qué pasa a la larga con la misma matriz repetida; $\sigma_1$ dice lo más que puede estirar un paso. Siempre $\rho \le \sigma_1$, y pueden ser muy distintos (matrices 4 y 5).
3. $\sigma_1(U) < 1$ garantiza que la señal se apaga, pase lo que pase con los datos. Es un techo, no un piso.
4. La saturación ($D_t$) sólo encoge: una memoria que se acomoda en un lugar estable apaga el gradiente.

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
