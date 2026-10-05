# Pendientes de BasicRNA

Nada de esto está decidido salvo donde se diga. Son huecos y planes anotados
para no volver a razonarlos desde cero. Los dos primeros esperan los comentarios
de los profesores del DASC.

La cuarta página, `4-Backprop.html`, ya está construida, y con eso sale de aquí
el pendiente que ocupaba este primer lugar. Las razones de cada decisión suya
—los dos relojes, por qué δ va en el nodo y el ajuste en la arista, el
presupuesto de color, la única escala que no es absoluta— viven en el comentario
de cabecera de ese archivo, que es su sitio: ahí las encuentra quien vaya a
tocarlo. Lo que quedó abierto al construirla está en el punto 3 de esta lista.

## 1. El corte de la función objetivo en `1-Perceptron.html`

Era prerrequisito de la cuarta página. Ya no lo es —la cuarta trae su propia
curva de pérdida por iteración—, y eso cambia el argumento pero no la
conclusión: sigue valiendo la pena, y ahora por una razón propia.

**El hueco.** Las tres primeras páginas miden con el contador de mal
clasificados, que es entero y plano. El alumno arrastra un peso medio recorrido,
la cifra no se inmuta, y después salta de golpe. Lo que vio fue un indicador que
se queda quieto mientras él se mueve, que es la lección contraria a la que hace
falta.

**Qué sería.** Al lado del contador, el error suave —la log-verosimilitud, que
es exactamente lo que minimiza el descenso por gradiente— y, del parámetro
seleccionado, su curva a lo largo de todo el recorrido del deslizador con la
posición actual marcada. El alumno arrastra, ve el contador estancado y la curva
bajando, y entiende que hay un fondo hacia el que conviene ir.

**Qué cambia ahora que existe la cuarta página.** Las dos curvas no son la misma
y conviene no confundirlas. La de la cuarta página es la pérdida **contra el
tiempo**: baja porque el algoritmo trabaja. La de la primera sería la pérdida
**contra un parámetro**, con la red quieta: un corte unidimensional del paisaje,
que no baja sola y que el alumno recorre con la mano. La primera enseña que hay
un fondo; la cuarta, que hay quien baje hacia él. En ese orden se leen mejor, y
es un argumento para hacerla antes que después.

**Dónde.** En `1-Perceptron.html`, no en una página nueva. Es donde el espacio
tiene tres parámetros y todo se puede decir en una frase, y donde el alumno
todavía tiene atención libre. En las otras dos competiría con lo que ya están
enseñando.

**Por qué es barato.** La curva se traza evaluando la misma red que ya se evalúa
para el mapa, sobre los mismos puntos, variando un solo parámetro. No hace falta
motor nuevo, ni gradiente, ni entrenamiento: es la función objetivo dibujada, no
optimizada.

**Lo que falta decidir.** Cuántos puntos tiene el corte y si se recalcula en cada
movimiento o sólo al seleccionar. Si vive en su propio lienzo pequeño o dentro
de la pista del deslizador. Si la segunda cifra se enseña desde el principio o
aparece después, como los botones. Y si conviene decir en clase que el contador
y el error suave pueden discrepar —bajar uno y subir el otro—, porque eso
también es cierto y también es materia.

## 2. Los cuarenta puntos de prueba no trabajan

**El hueco.** Las cuatro páginas parten los 200 puntos en 160 de entrenamiento y
40 de prueba, dibujan los primeros en círculo y los segundos en cruz, y cuentan
las dos particiones por separado. Pero todas las soluciones horneadas dan cero y
cero en las dos, así que la partición de prueba nunca dice nada distinto de la
de entrenamiento. La distinción ocupa dos renglones de leyenda, una cifra en el
contador y un símbolo en el mapa, y no enseña nada.

**Las dos salidas.** Una es quitarla: un solo conjunto, un solo contador, y la
generalización se trata donde de verdad se pueda mostrar. La otra es hacer que
se gane su lugar, y para eso hay que construir un caso donde la prueba y el
entrenamiento discrepen a ojo: menos puntos de entrenamiento, o con ruido en las
etiquetas, o con los conjuntos muestreados de regiones que no se solapan del
todo.

**Lo que destraba la cuarta página.** Antes había que medir si el sobreajuste se
alcanza moviendo parámetros de uno en uno, y eso era dudoso. Ahora hay un sitio
donde la red entrena sola y el sobreajuste puede aparecer por su cuenta, así que
el experimento es concreto y se puede correr: dejar `4-Backprop.html` avanzando
bastante más allá de la convergencia y ver si el contador de prueba se separa
del de entrenamiento con estos datos. Con la holgura que hay entre el disco y el
anillo es posible que no se separe nunca, y en ese caso haría falta ensuciar las
etiquetas o recortar el entrenamiento. Es una medición, no una discusión.

**Cuál prefiero.** Ninguna de las dos sin medir antes. Lo que sí es claro es que
el estado actual es el peor de los tres: paga el costo visual de la distinción
sin cobrar el beneficio.

## 3. Lo que dejó abierto la cuarta página

Ninguno de estos es un defecto que impida usarla; son cosas que conviene mirar
con los colegas antes de darla por cerrada.

**La única escala que no es absoluta.** El grosor del pulso del ajuste compara
los diecisiete ajustes de ese paso entre sí, no contra el tamaño del peso. Tenía
que ser así para que se viera algo con centésimas, está declarado en la leyenda
y el panel da además el mayor |Δθ| en número. La pregunta es si con eso basta o
si hace falta algo más en el dibujo mismo, porque una escala relativa sin aviso
es exactamente el tipo de cosa que el alumno se lleva mal aprendida.

**Con tanh no se ve nada equivalente a la neurona muerta.** La ganancia que sale
gratis es de la ReLU: φ′ vale 0 o 1 y la neurona con z ≤ 0 se apaga a la vista.
Con tanh φ′ nunca es cero, sólo pequeño, así que el gradiente se desvanece sin
que la página lo muestre. Es el fenómeno que más importa de las dos y es el que
no tiene dibujo. Podría ser el grosor del anillo —que ya es |δ|— dicho de otro
modo, o una marca cuando φ′ cae por debajo de algo; o podría ser deliberado
dejarlo para TalleRNA. No está decidido.

**El tope del grosor.** Se midió que con η = 0.05 ningún peso pasa de 6 antes de
llegar a cero errores, así que la escala fija no estorba en el uso previsto.
Entrenando mucho más allá sí lo pasan y esas líneas se dibujan todas iguales. Es
el precio de que un grosor signifique lo mismo en las cuatro páginas, y me
parece el precio correcto, pero conviene saberlo antes de que alguien lo
descubra en clase.

**El tamaño de la tanda.** Catorce apretones de +100 con ReLU y veinticuatro con
tanh. Un botón más grande —o uno de «entrenar hasta el final»— lo haría cómodo y
al mismo tiempo borraría la lección, que es justamente cuántos pasos hacen falta.
Se quedó en +10 y +100 por eso. Si en clase resulta insufrible, la salida menos
mala es un +500, no un botón que llegue solo al final.

**La frontera enredada de los primeros pasos.** Los puntos de cruce se ordenan
por ángulo alrededor de su centroide, lo que supone un lazo estrellado. Con los
pesos iniciales no lo es y la curva se cruza a sí misma durante unos cientos de
iteraciones. Dice la verdad sobre la red que hay, y se arregla sola, así que se
dejó; sustituirlo por un seguimiento de contorno en regla es trabajo real y
beneficia a las cuatro páginas, no sólo a ésta.

**Las tres primeras ya caben proyectadas** en 1196 × 940, con 20 px de margen
lateral: terminan en 908, 903 y 927 px (medido con Claude Code, en Chrome).
Por homogeneizar: su margen de arriba y de abajo (28 y 48 px) sigue siendo
distinto del de la 4 a la 8 (16 y 22).

## 4. Páginas 5 y 6: una CNN mínima

Decidida en lo esencial; lo abierto está al final. Son dos páginas:
`5-Convolucion.html` usa una red ya entrenada y `6-BackpropCNN.html` la
entrena. En ninguna se mueven pesos a mano: en la 5 el alumno cambia la
entrada y puede girar el filtro, y en la 6 sólo cambia los hiperparámetros.
El primer diseño, con máximo global y pesos movidos a mano, se cambió por éste
porque con 27 parámetros ya no se mueve a mano, y porque así la capa completa
se entrena de forma visible.

**Qué red.** Imagen 6×6 → un solo filtro 3×3 con sesgo b, aprendido, sin
relleno → mapa 4×4 → activación → 16 nodos → salida σ(Σ u_p·a_p + c): 27
parámetros, los nueve del filtro, b, los dieciséis u_p y c. Un solo filtro
porque con uno se resuelve y ya enseña las dos ideas: el mismo filtro en
todas partes, y cada celda del mapa mira una zona de la imagen. La etiqueta
es vertical = 1 y, como en la tercera y la cuarta página, ŷ ≥ 0.5 es clase 1.

**Datos.** 200 imágenes, un segmento de longitud 3 vertical u horizontal,
con una sola intensidad por imagen en [0.75, 1], sobre fondo con ruido
uniforme en [0, 0.25]. El trazo no toca las columnas 0 y 5: los horizontales
empiezan en la columna 1 o 2. Sin ese margen esos trazos serían invisibles:
medido con los pesos de la página 5, un vertical de 3 píxeles pintado en la
columna 0 o en la 5 da la misma salida que el lienzo vacío, porque ninguna
ventana lo tiene bajo su columna central y la ReLU apaga las celdas que lo
ven. Estratificados: 80 verticales y 80
horizontales en las 160 de entrenamiento, 20 y 20 en las 40 de prueba, en
orden mezclado dentro de cada partición.

**Por qué pertenece aquí.** Cada una de las dos ideas tiene su gesto: la
ventana recorre la imagen con el mismo cuadrito, y señalar una celda o un
nodo enciende la zona que mira. Y el perceptrón de la primera página no puede
con estos datos: las tres columnas de un cuadrado 3×3 suman lo mismo que sus
tres filas, así que ninguna función lineal de los píxeles separa las clases.
La imposibilidad se atribuye a ese argumento aplicado a la regla que genera
los datos (verificado numéricamente, 2.7e-15), no al contador: sobre la
muestra, la programación lineal con los 36 píxeles empieza a fallar desde
unas 76 imágenes, y eso mide la capacidad lineal, cuántas imágenes alcanza a
separar una función lineal con esa cantidad de rasgos, no la estructura del
problema.

**Cómo se ve.** Plana y con poco detalle numérico: los números los enseña la
cuarta página. Una ventana verde sobre la imagen; el filtro como un cuadrito
3×3 aparte, en azul lo positivo y en rojo lo negativo, que es el código del
directorio; el mapa 4×4; y los 16 nodos en cuatro grupos de cuatro, con una
línea punteada de cada fila del mapa a su grupo: aplanar es poner las filas
una debajo de otra. Señalar un nodo o una celda ilumina la celda, el nodo y
la ventana. Un nodo vacío está apagado, porque la ReLU lo dejó en cero, y su
línea queda tenue. El grosor de las líneas a la salida es |u| contra el tope
fijo 6, como en la cuarta página. El recorrido del filtro es rápido.

**Página 5, la red ya entrenada.** El alumno cambia la entrada: una
galería de imágenes y un lienzo 6×6 donde pinta su propio trazo. La imagen
del diagrama es el lienzo, y un píxel pintado vale 1 o 0, sin intensidades
intermedias. La salida pregunta «¿Hay un trazo vertical?» y contesta «Sí» o
«No», nunca «horizontal»: al lienzo vacío le contesta «No». Trae dentro
los pesos elegidos así: de los arranques con datos estratificados que
llegaron a cero, el vector con mayor confianza mínima entre los que tienen
todos sus parámetros con |θ| ≤ 6. La confianza mínima es la menor
probabilidad que la red da a la clase correcta entre las 200 imágenes. Es el
arranque 50128, con ReLU y η = 0.1, en la época 58: llegó a cero en la 8 y
siguió 50 más. Confianza mínima 0.989, mayor |θ| 5.86, que es el de c, y
cero errores en las 160 y en las 40. El vector completo lo da
`elegir-pesos.js`, junto al reporte que se cita abajo.

**Girar el filtro.** Un clic en el cuadrito del filtro lo gira y otro lo
regresa; en el código es trasponer, que es lo que corresponde a trasponer la
imagen. Sólo cambian los nueve números del filtro: b, los u_p y c se quedan
iguales. La razón: el filtro es lo que la red busca, y cambiando el filtro la
misma red busca otra cosa, sin reentrenar. La pregunta pasa a «¿Hay un trazo
horizontal?», y los marcos de la galería y el contador se calculan contra
ella. Medido con el arnés sobre el conjunto de la página: 29 errores en las
160 y 9 en las 40, que son exactamente las 38 horizontales de las filas 0 y 5,
por la misma razón que un vertical en la columna 0 o 5: los datos no son
simétricos, porque los verticales nunca tocan esas columnas y los horizontales
sí tocan esas filas. Con trazos limpios en 1, las 16 horizontales de las filas
1 a 4 dicen «Sí», las 8 de las filas 0 y 5, las 24 verticales y el lienzo
vacío dicen «No». La página verifica al montar que trasponer dos veces
devuelve el filtro y que las mal clasificadas son exactamente esas 38.

**Página 6, entrena.** Un paso animado —de ida, el recorrido, el mapa, el
aplanado, la salida y el error; de regreso, los grosores y el filtro— y tandas
de +1 época y +10 épocas (Un paso ya es una imagen). Una época es una pasada
por las 160 imágenes, cada una exactamente una vez, en orden barajado de nuevo
cada época; es distinto del sorteo con reposición de la cuarta página, y a
propósito. La tasa va en [0.01, 0.20] y empieza en 0.05. Un interruptor entre
ReLU y lineal: con lineal la red entera queda lineal en los píxeles y nunca
llega a cero, por el argumento del cuadrado. La curva de la pérdida, como en
la cuarta página. Sin galería y sin botones Vertical/Horizontal: cada imagen
sale del orden barajado de la época.

**Medido en JavaScript.** La fuente es
`../BasicRNA-trabajo/entrena-cnn/reporte.txt`, fuera del repositorio. Con
ReLU y datos estratificados, 200 arranques por tasa, semilla de datos
20260928 y tope de 100 épocas. Llegar es tener cero errores en las 160 y en
las 40 al final de una época. Un arranque apagado termina con todas las
z_p ≤ 0 en las 200 imágenes: todos los nodos en cero y la misma salida para
todas.

```
tasa    llegan    época de llegada (mediana)    apagados
0.01    75/200    36                             4
0.02    83/200    21                             4
0.05    79/200     8                             8
0.1     68/200     5                            14
0.2     44/200     3                            48
```

Con lineal no llega ninguno, en ninguna tasa. Los apagados crecen con la
tasa, y ésa es la razón para exponerla; enlaza con las neuronas muertas de la
cuarta página. Entre el 25 % y el 51 % de los que llegan a cero vuelven a
tener errores si se sigue entrenando 50 épocas más (con los estratificados,
del 25 % al 46 %), y por eso la curva de la pérdida es necesaria. El éxito
varía con el conjunto de datos: con ReLU y η = 0.05, en 21 conjuntos, va de
46 a 96 de 200 con sorteo libre, donde la semilla 20260928 da 96, la mejor de
las 21 y no la típica (mediana 75); con los estratificados va de 60 a 92, y
la 20260928 da 79, cerca de la mediana 76. La prueba casi nunca se separa del
entrenamiento: con η = 0.05, 9 de 200 arranques estratificados tienen en
alguna época cero errores en entrenamiento y alguno en prueba (8 de 200 con
sorteo libre).

**El arranque por omisión.** Abierto. Los diez candidatos del reporte, con
ReLU, η = 0.05 y datos estratificados: su vector inicial no da la misma clase
a las 200 imágenes, con ReLU llegan a cero entre las épocas 5 y 15, y con el
mismo vector y lineal se quedan en su meseta. Son 50000 (llega en la época 9),
50002 (10), 50005 (8), 50013 (10), 50014 (6), 50020 (10), 50036 (11), 50037
(5), 50041 (5) y 50044 (6). La elección final se hace viéndolos en la página.
La página 6 lleva provisionalmente el 50013. Su vector inicial coincide con el
del arnés (50 de 1000 semillas dan a las 200 imágenes la misma clase, como
cuenta el reporte), pero el orden de las épocas no se cotejó con el arnés: con
el barajado de la página el 50013 llega en la época 9, no en la 10 del
reporte. Falta cotejar el orden contra entrena-cnn/ y elegir. Esto se midió en
el chat, no con Claude Code.

**El cambio del filtro en el regreso.** Decidido y construido en la 6: la
ventana regresa sólo por los nodos encendidos; cada celda del filtro muestra
su cambio con un cuadro interior violeta u ocre, y b con un punto; una sola
escala para los 27. Las razones están en el comentario de cabecera de
6-BackpropCNN.html.

**El tope 6.** Abierto. Con η ≤ 0.05 ningún peso lo rebasa al llegar a cero
(el mayor, 5.52); con 0.1 y 0.2 lo rebasan al llegar 11 de los 763 arranques
que llegan, 5 de ellos estratificados, hasta 13.4. Cincuenta épocas después
la mediana del mayor |θ| está entre 3.5 y 6.6 según la tasa: entrenando de
más, algunas líneas saturan.

**La partición de prueba.** Abierto: es el problema del punto 2, y las
páginas CNN no lo resuelven.

**La 6 proyectada.** Abierto: no se ha proyectado a 1196×940; la altura total
(unos 840 px) es estimada, y el texto a la derecha de la salida deja unos 4 px
de margen en el lienzo de 708.

**Lo que obliga a cambiar en el README.** La portada y el index.html necesitan
dos miniaturas más.

## 5. Páginas 8 y 9: una red recurrente mínima

`8-Recurrente.html` ya está construida: una neurona recurrente escalar de
cinco parámetros que el alumno mueve a mano, sobre tiras de píxeles, con tres
tareas (el último, la mayoría, el primero) y un interruptor entre lineal y
tanh. Las razones de cada decisión suya viven en su comentario de cabecera.

**Página 9, construida.** `9-BackpropTiempo.html` entrena la red de la 8 por
retropropagación en el tiempo, de a una tira. Las razones de cada decisión suya
y lo que se midió viven en su comentario de cabecera.

**Por homogeneizar con la 8.** La 9 oculta los círculos de los radios para que
la barra quepa en un renglón. Y escribe bajo cada estado el orden de magnitud
de |δ|, excepción deliberada a la regla de la 8 de que ningún dato aparece dos
veces.

**Respuesta parcial al punto 2.** La prueba de la 8 son 40 tiras de 9
píxeles, más largas que las 128 de 7 de entrenamiento, así que mide algo que
el entrenamiento no mide: funcionar con otra longitud. En la mayoría sí
distingue: la ventana de u sin errores es más angosta en la prueba, y con
u = 0.94 el contador de entrenamiento dice 0 y el de prueba 1. En el último y
el primero no: las ventanas salen iguales o más anchas y la prueba no añade
nada. Las cifras son las de la sección LOS DATOS de la cabecera; no se
volvieron a medir con Claude Code.

## 6. Página 10: atención

Especificada en `ESPEC-10-Atencion.md` y todavía sin construir. Sus decisiones
abiertas están en la sección 13 de ese archivo, «Pendiente de decidir». El
número ya no: es el 10 desde que la página de la neurona en el tiempo entró
como séptima y recorrió las que seguían.

## 7. Páginas recurrentes: una neurona en el tiempo

La página está construida y es la séptima, `7-NeuronaEnElTiempo.html`. Lo que
sigue es por qué existe, qué quedó decidido al construirla y qué falta. Nada de
esto lleva cifras medidas.

**El hueco que llena.** La octava dibuja la red desenrollada, una copia de la
celda por píxel. El alumno llega de las páginas 2 a 6, donde cada círculo es una
neurona distinta y el eje horizontal son las capas. Ahí los círculos son la
MISMA neurona en momentos distintos y el eje horizontal es el tiempo, y el
dibujo no lo dice en ninguna parte. A eso se suman los subíndices: en la segunda
página h1 a h4 nombran cuatro neuronas distintas, así que si las copias se
rotularan h1 a h7 el alumno leería siete neuronas. La séptima lo dice antes, con
dibujo: una neurona arriba, dibujada una sola vez, y abajo el registro de lo que
hizo, que crece una columna por momento.

**Cómo quedó.** Sólo de conceptos: sin tareas, sin contadores, sin deslizadores
y sin entrenamiento, y el único gesto es avanzar un momento. Arriba la neurona
plegada, rotulada como UNA neurona, con las tres entradas del perceptrón de la
primera página: el píxel del momento, su propia salida anterior —el lazo— y el 1
del sesgo. Abajo la tira y el botón de avanzar; cada pulsación agrega una
columna, y lo que viaja por el lazo viaja por la flecha que llega a la columna
siguiente, porque son la misma flecha. Bajo cada columna, «momento t»; ninguna
neurona lleva subíndice; el estado se escribe. El contraste recuerda / olvida es
el centro, con pesos fijos y sin deslizador: recuerda es la solución lineal de la
mayoría de la octava (w = 2, b = −1, u = 1) y el estado es un contador entero;
olvida es la misma con el lazo cortado (u = 0), y el estado vale +1 o −1. Las
razones de cada decisión suya viven en su comentario de cabecera.

**Lo que se decidió al construirla.** Eran los cuatro puntos abiertos del plan,
y quedaron como Claude los propuso. La disposición es la neurona fija arriba y el
registro creciendo abajo, no la neurona avanzando por la tira y dejando copias
detrás. El plegado es vertical, con la entrada arriba, igual que cada columna del
desenrollado, y no de izquierda a derecha como las páginas 1 a 6. El alumno pinta
los píxeles y elige la longitud, 7 o 9, para que se vea que la misma neurona
sirve para cualquiera. Y la página lleva neurona de salida: con recuerda pregunta
por la mayoría y con olvida por el último píxel. Miguel las aprobó al revisar la
página construida.

**Lo que sigue abierto.**

a. Adaptar la novena. La octava ya está rehecha: lleva la red dibujada una vez
arriba a la izquierda, junto al deslizador, y el desenrollado abajo (D4). La
novena conserva la figura que la octava tenía antes, así que las dos páginas
recurrentes que entrenan y que se mueven a mano ya no comparten dibujo. Qué
hereda la novena —la red dibujada una vez, y si sobre ella se escribe algo del
regreso del error— se decide después.

b. Qué hacer contra el desvanecimiento de la señal de error. La novena lo mide y
lo escribe sobre cada flecha; qué se hace al respecto se discute después.

## Lo que no está pendiente

Que la tercera página se preste a un estudio más profundo —activaciones
aprendidas, búsqueda sobre el espacio de funciones, la conexión con KAN— no es
un pendiente de BasicRNA. Eso vive en la sección de redes especiales, después de
TalleRNA, y estas páginas no tienen que prepararlo más de lo que ya lo hacen.

Tampoco lo es abrir más hiperparámetros en la cuarta página. La frontera del
directorio es que aquí no se configura: momento, tamaño de lote, épocas e
inicialización son de TalleRNA. Si se abren aquí, esta página ya es TalleRNA y
el directorio pierde su límite.

Tampoco lo es simular varias capas convolucionales. Lo que estas páginas
enseñan es que un kernel detecta un rasgo básico, y una capa con un filtro
alcanza para eso.

## Pendientes de mantenimiento

- `README.md` está sobre su tope de 40 líneas.
- `README.md` dice «siete páginas» y no menciona la 8.
