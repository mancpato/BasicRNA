# El lazo como matriz — la señal de gradiente con dos neuronas

**Lugar en la serie:** apoya la página 11, `11-LazoComoMatriz.html`, que va después de la de varias neuronas (la 10) y antes de la de atención (la 12). Es el puente: la página pasa de números a vectores, de vectores a matrices, y de las matrices a la geometría de lo que hacen.

## Por qué existe

En la página 9 la red tenía una sola neurona y la señal de gradiente retrocedía multiplicándose, en cada paso, por un número. Con dos neuronas ese número se vuelve una matriz de $2 \times 2$, y aparecen tres cosas nuevas:

1. **La dirección.** La señal ya no es un número sino una flecha en el plano. La matriz la estira en unas direcciones, la encoge en otras y puede girarla.

2. **Dos medidas en lugar de una.** Con una neurona bastaba ver si el factor era mayor o menor que 1. Con una matriz hay dos medidas distintas:
   * el **radio espectral** $\rho$ (el mayor tamaño de sus eigenvalores), que dice qué pasa *a la larga* cuando la misma matriz se repite muchas veces;
   * el **mayor valor singular** $\sigma_1$, que dice lo *más* que la matriz puede estirar una flecha en *un solo paso*.

   Siempre $\rho \le \sigma_1$. En algunas matrices son iguales; en otras son muy distintos, y ahí está la sorpresa.

3. **El factor cambia en cada momento.** La derivada de la activación, $\phi'$, depende del estado de cada neurona, así que la señal se multiplica por un producto de matrices *distintas*. Entonces los eigenvalores dejan de predecir lo que pasa, y sólo $\sigma_1$ sigue dando una garantía.

## De un número a una matriz

| | Una neurona (página 9) | Dos neuronas (esta página) |
|---|---|---|
| Memoria | número $h_t$ | vector $h_t$ (2 componentes) |
| Lazo | número $u$ | matriz $U$ ($2 \times 2$) |
| Pendiente de la activación | número $\phi'(z_t)$ | matriz diagonal $D_t$ |
| Señal de gradiente | número $\delta_t$ | vector $\delta_t$ |
| Factor de retroceso | $f_t = \phi'(z_t)\, u$ | $F_t = D_t\, U^T$ |
| $\rho$ y $\sigma_1$ | los dos valen $\lvert u \rvert$ | pueden ser muy distintos |
| Se apaga con seguridad si | $\lvert u \rvert < 1$ | $\sigma_1(U) < 1$ |

## La red

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

**Momentos y pasos hacia atrás.** $T = 40$, y con 40 momentos hay 39 pasos hacia atrás: el contador $k$ va de 0 a 39, y $t = T - k$ es el momento por el que va el retroceso.

## La señal de gradiente

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

## Dónde empieza la señal

En el último momento, con la sigmoide de salida y la entropía cruzada como pérdida,

$$\begin{bmatrix} \delta_{T,1} \\ \delta_{T,2} \end{bmatrix} = \begin{bmatrix} \phi'(z_{T,1}) & 0 \\ 0 & \phi'(z_{T,2}) \end{bmatrix} \begin{bmatrix} v_1 \\ v_2 \end{bmatrix} (\hat y - y)$$

donde $y$ es la respuesta correcta. En una red que se entrena, la dirección de $\delta_T$ la deciden $v$ y $D_T$. En esta página no se entrena: $\delta_T$ **lo elige el alumno** (arrastrando la flecha) o lo pone la matriz preparada, porque lo que se estudia es qué hace el lazo con cada dirección. Todas las flechas iniciales de las matrices preparadas miden 1.

## Medir cuánto estira una matriz

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

## La desigualdad fundamental

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

## Las matrices preparadas

Las tres primeras son **normales** ($U U^T = U^T U$): para ellas $\sigma_1 = \rho$ y las dos medidas cuentan la misma historia. Las dos últimas no son normales, y ahí aparece la brecha. Además, las matrices 1, 3 y 4 tienen el mismo $\rho = 0.9$, para poder compararlas.

| Matriz | Eigenvalores | $\rho$ | $\sigma_1$ | $\sigma_2$ | ¿Normal? | $\delta_T$ inicial |
|---|---|---|---|---|---|---|
| 1. Diagonal | 0.9 y 0.5 | 0.9 | 0.9 | 0.5 | sí | $(0.707,\ 0.707)$ |
| 2. Simétrica | 1.2 y 0.4 | 1.2 | 1.2 | 0.4 | sí | $(1,\ 0)$ |
| 3. Giro | $0.9\,e^{\pm i\,30^\circ}$ | 0.9 | 0.9 | 0.9 | sí | $(1,\ 0)$ |
| 4. Joroba | 0.9 (doble) | 0.9 | ≈ 1.530 | ≈ 0.530 | no | $(1,\ 0)$ |
| 5. Nilpotente | 0 (doble) | 0 | 4 | 0 | no | $(0.707,\ 0.707)$ |

($0.707 \approx \sqrt{2}/2$, para que la flecha mida 1.)

Las cifras en modo lineal que se citan abajo salen de la fórmula cerrada de cada matriz y se confirmaron iterando la matriz paso a paso, con una diferencia peor de $9 \times 10^{-16}$; las de modo $\tanh$ las midió Claude Code con un arnés en Node, `binary64`, con el empuje inicial $a = 0.2$, el deslizador de la página que escala $W$ y $b$. La página las vuelve a comprobar al montar, y no monta si alguna falla.

### 1. Matriz diagonal

$$U = U^T = \begin{bmatrix} 0.9 & 0 \\ 0 & 0.5 \end{bmatrix}$$

* **Modo lineal.** Las dos componentes evolucionan sin mezclarse: la primera se multiplica por 0.9 y la segunda por 0.5 en cada paso.
$$\delta_{T-k} = \begin{bmatrix} 0.707 \cdot 0.9^k \\ 0.707 \cdot 0.5^k \end{bmatrix}$$
A los 5 pasos, $\delta_{T-5} \approx (0.418,\ 0.022)$. La flecha no gira, pero queda casi sobre el eje de la neurona 1, porque esa componente sobrevive más.
* **Modo tanh.** $D_t$ y $U^T$ son diagonales, así que las neuronas siguen sin mezclarse; cada componente sólo se encoge un poco más.
* **Plano.** La elipse tiene sus ejes sobre los ejes del plano (semiejes 0.9 y 0.5). Esos dos ejes son las direcciones que no giran.

### 2. Matriz simétrica

$$U = U^T = \begin{bmatrix} 0.8 & 0.4 \\ 0.4 & 0.8 \end{bmatrix}$$

Eigenvectores: $(1,1)$ con eigenvalor 1.2, y $(1,-1)$ con eigenvalor 0.4.

* **Modo lineal.** La flecha inicial se descompone en esas dos direcciones:
$$\begin{bmatrix} 1 \\ 0 \end{bmatrix} = \frac{1}{2} \begin{bmatrix} 1 \\ 1 \end{bmatrix} + \frac{1}{2} \begin{bmatrix} 1 \\ -1 \end{bmatrix} \quad\Longrightarrow\quad \delta_{T-k} = \frac{1}{2}\,(1.2)^k \begin{bmatrix} 1 \\ 1 \end{bmatrix} + \frac{1}{2}\,(0.4)^k \begin{bmatrix} 1 \\ -1 \end{bmatrix}$$
Primeros pasos:
$$\begin{bmatrix} 1 \\ 0 \end{bmatrix} \to \begin{bmatrix} 0.8 \\ 0.4 \end{bmatrix} \to \begin{bmatrix} 0.8 \\ 0.64 \end{bmatrix} \to \cdots$$
La parte de 0.4 desaparece pronto y la flecha queda sobre $(1,1)$, creciendo: largo ≈ 4.38 a los 10 pasos y 866 a los 39, el último.
* **Modo tanh.** Aquí la historia se voltea. El eigenvalor 1.2 empuja la memoria hacia la saturación. Por ejemplo, sin entrada ni sesgo, una vez que la memoria sale de cero se asienta cerca de $h \approx (0.66,\ 0.66)$, donde
$$D_t \approx \begin{bmatrix} 0.57 & 0 \\ 0 & 0.57 \end{bmatrix}, \qquad F_t \approx 0.57\, U^T$$
y los eigenvalores de $F_t$ quedan en $0.57 \times 1.2 \approx 0.68$ y $0.57 \times 0.4 \approx 0.23$: la señal se apaga. Con la entrada de la página la caída es más fuerte todavía: desde $\delta_T = (1, 0)$ la señal cae a 0.34 en el primer paso y a $7 \times 10^{-7}$ en el paso 20, contra los 866 del modo lineal.

  **Lección:** una memoria que se acomoda en un lugar estable (si la empujas un poco, regresa) no deja pasar el gradiente lejos. La misma estabilidad que evita que la memoria se descontrole hace que la señal de regreso muera.
* **Plano.** Elipse con ejes en las diagonales $(1,1)$ y $(1,-1)$, semiejes 1.2 y 0.4.

### 3. Matriz de giro

$$U = 0.9 \begin{bmatrix} \cos 30^\circ & -\sin 30^\circ \\ \sin 30^\circ & \cos 30^\circ \end{bmatrix} \approx \begin{bmatrix} 0.779 & -0.450 \\ 0.450 & 0.779 \end{bmatrix}, \qquad U^T \approx \begin{bmatrix} 0.779 & 0.450 \\ -0.450 & 0.779 \end{bmatrix}$$

Sus eigenvalores son complejos, $0.9\,e^{\pm i\,30^\circ}$: no hay ninguna dirección que se quede sin girar.

* **Los dos giros.** $U$ gira $30^\circ$ en sentido contrario a las manecillas del reloj; $U^T$ gira $30^\circ$ **en el sentido de las manecillas**. La memoria y la señal de gradiente giran en sentidos opuestos: es la transpuesta, vista.
* **Modo lineal.**
$$\begin{bmatrix} 1 \\ 0 \end{bmatrix} \to \begin{bmatrix} 0.779 \\ -0.450 \end{bmatrix} \to \cdots$$
La flecha da una vuelta completa cada 12 pasos y se encoge por 0.9 en cada uno: al terminar la primera vuelta mide $0.9^{12} \approx 0.28$.
* **Modo tanh.** $D_t$ deforma el círculo en elipse; el giro deja de ser parejo y la flecha se encoge más rápido.
* **Plano.** $\sigma_1 = \sigma_2 = 0.9$, así que la "elipse" es un círculo de radio 0.9: la matriz no prefiere ninguna dirección.

### 4. Matriz no normal: la joroba

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
* **Modo tanh.** La joroba se achata: con el empuje inicial sube sólo hasta 1.26, contra 3.89 en lineal. Cuánto se achate depende de qué tan saturadas estén las neuronas.
* **Plano.** Elipse alargada, de semiejes ≈ 1.530 y ≈ 0.530, aunque el eigenvalor sea 0.9.

### 5. Matriz nilpotente: explosión y desaparición

$$U = \begin{bmatrix} 2 & -2 \\ 2 & -2 \end{bmatrix}, \qquad U^T = \begin{bmatrix} 2 & 2 \\ -2 & -2 \end{bmatrix}$$

$\rho = 0$ (sus dos eigenvalores son 0), pero $\sigma_1 = 4$. Además, la matriz multiplicada por sí misma se anula:

$$U^T U^T = \begin{bmatrix} 0 & 0 \\ 0 & 0 \end{bmatrix}$$

* **Por qué.** $U^T$ se escribe como columna por renglón:
$$U^T = \begin{bmatrix} 2 \\ -2 \end{bmatrix} \begin{bmatrix} 1 & 1 \end{bmatrix}, \qquad U^T \begin{bmatrix} \delta_1 \\ \delta_2 \end{bmatrix} = (\delta_1 + \delta_2) \begin{bmatrix} 2 \\ -2 \end{bmatrix}$$
Primero suma las dos componentes y después reparte esa suma en la dirección $(2, -2)$. Pero esa dirección suma cero: $2 + (-2) = 0$. En el siguiente paso no queda nada. Dicho de otro modo: $U^T$ manda todo a la línea de $(1,-1)$, y esa misma línea es la que manda a cero.
* **Modo lineal**, desde $\delta_T = (0.707,\ 0.707)$:
$$\begin{bmatrix} 0.707 \\ 0.707 \end{bmatrix} \to \begin{bmatrix} 2.828 \\ -2.828 \end{bmatrix} \to \begin{bmatrix} 0 \\ 0 \end{bmatrix}$$
La señal se multiplica por 4 en un paso y desaparece en el siguiente.
* **Modo tanh.** Entre los dos pasos se mete $D_t$, con números $d_1$ y $d_2$ en su diagonal:
$$\begin{bmatrix} d_1 & 0 \\ 0 & d_2 \end{bmatrix} \begin{bmatrix} 2 \\ -2 \end{bmatrix} = \begin{bmatrix} 2 d_1 \\ -2 d_2 \end{bmatrix}, \qquad \text{que suma } 2(d_1 - d_2)$$
Sólo se anula si las dos neuronas tienen la misma pendiente. Con $\tanh$ la señal ya no desaparece: la frena la diferencia entre las dos pendientes. Con el empuje inicial, la señal pasa de 3.53 a 0.91 en el segundo paso, en vez de a cero.
* **Plano.** La elipse se aplasta en un segmento de semilargo 4 en la dirección $(1,-1)$; en el paso siguiente, en un punto.

*Nota para la construcción:* como los dos renglones de $U$ son iguales, $z_{t,1} - z_{t,2} = (w_1 - w_2)\,x_t + (b_1 - b_2)$ no depende de la memoria. Si las dos neuronas recibieran la misma entrada y el mismo sesgo, tendrían siempre la misma pendiente y la matriz seguiría siendo nilpotente aun con $\tanh$. Por eso los valores de $W$ y $b$ de la página son distintos para cada neurona.

## Hacia la atención

Todo el problema de esta página viene de que la señal tiene que atravesar $k$ factores en fila para llegar $k$ pasos atrás. La pregunta que abre la página de atención, la 12: ¿y si la salida pudiera ir a buscar directamente cualquier memoria $h_t$, en lugar de que todo pase por la última?

## Lo que el alumno se lleva

1. La red avanza con $U$; la señal de gradiente retrocede con $U^T$: las mismas líneas, al revés.
2. $\rho$ dice qué pasa a la larga con la misma matriz repetida; $\sigma_1$ dice lo más que puede estirar un paso. Siempre $\rho \le \sigma_1$, y pueden ser muy distintos (matrices 4 y 5).
3. $\sigma_1(U) < 1$ garantiza que la señal se apaga, pase lo que pase con los datos. Es un techo, no un piso.
4. La saturación ($D_t$) sólo encoge: una memoria que se acomoda en un lugar estable apaga el gradiente.
