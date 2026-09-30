# BasicRNA

Siete páginas sueltas para las primeras clases sobre redes neuronales. Cada una
es un archivo HTML único, sin dependencias y sin build: se abre con doble clic y
dibuja con canvas 2D.

![Las cuatro páginas: una neurona con su recta separadora, la red 2→4→1 con diecisiete deslizadores, la misma red con una activación distinta por neurona, y la animación de un paso de retropropagación](img/portada.png)

Son deliberadamente pobres en perillas. La frontera de este directorio no es que
aquí no se entrene, sino que aquí no se configura: las tres primeras páginas no
entrenan —el alumno mueve los parámetros a mano, uno a la vez, y ve qué le pasa
a la frontera— y la cuarta entrena exponiendo un solo hiperparámetro, la tasa de
aprendizaje, que es el único cuyo efecto la animación vuelve visible. Momento,
tamaño de lote, épocas e inicialización se quedan en TalleRNA y las demás
herramientas de las que este directorio es hermano. Si se abrieran aquí, la
cuarta página ya sería TalleRNA.

## Las siete páginas, en orden

El orden importa: cada una deja una pregunta que contesta la siguiente.

**`1-Perceptron.html` — los tres parámetros del perceptrón.**
Una neurona con salida sigmoide. Cada parámetro hace algo que se puede decir en
una frase: los dos pesos giran la recta, el sesgo la desplaza. El alumno llega a
cero errores a mano sin demasiado esfuerzo, y sale sabiendo lo que se siente
entender un parámetro.

**`2-EditParam.html` — red 2→4→1, diecisiete parámetros.**
La misma mecánica con una capa oculta de cuatro neuronas y un interruptor entre
ReLU y tanh. Los datos son un disco contra el anillo que lo rodea, que ninguna
recta separa: la neurona sola de la página anterior no podría con ellos. Y aquí
la intuición ya no alcanza —mover un peso cambia el mapa de un modo que nadie
anticipa—, que es el punto de tenerla.

**`3-EditActivFun.html` — una función por neurona.**
La misma red, pero la activación se elige neurona por neurona entre seis. No son
un catálogo de fórmulas sino una casilla cada una, y la casilla se ve en el
mapa. El tercero de sus conjuntos de puntos es el que decide: con franjas, una
sola neurona seno lo resuelve y cuatro ReLU no llegan nunca.

**`4-Backprop.html` — quién acomoda los diecisiete.**
La red y los datos de la segunda página, pero aquí no se mueve nada a mano: se
ve cómo los mueve el algoritmo, un punto de entrenamiento a la vez. Es la
respuesta a la pregunta con la que uno se queda después de pelearse con
diecisiete deslizadores. Ver un paso y ver que la red aprende están separados
por tres órdenes de magnitud, así que hay un botón que anima el paso completo y
dos que lo repiten diez y cien veces con la animación apagada: un solo
mecanismo, que es lo que hay que poder explicar.

**`5-Convolucion.html` — la CNN más chica que todavía enseña algo.**
Una imagen de 6×6 con un trazo de tres píxeles, vertical u horizontal, y un
solo filtro 3×3 que la recorre: la misma ventana en todas partes, y cada
celda del mapa 4×4 mira una zona de la imagen. La red ya viene entrenada y
el alumno sólo cambia la entrada: elige una imagen de la galería o pinta la
suya. Puede además girar el filtro con un clic, y la misma red pasa a buscar
trazos horizontales sin reentrenar.

**`6-BackpropCNN.html` — cómo aprende el filtro.**
La red de la quinta, entrenada a la vista, una imagen a la vez. Es la
cuarta página aplicada a una capa convolucional: el error se calcula en la
salida y regresa nodo por nodo, y la ventana vuelve por las posiciones cuyo
nodo quedó encendido. El filtro es uno solo, así que su cambio es la suma de
lo que pide cada posición; los nodos que la ReLU apagó no aportan nada, y eso
se ve. Los controles son los de la cuarta: un paso animado, tandas de una y
diez épocas, la tasa y un interruptor entre ReLU y lineal. Con lineal nunca
llega a cero errores, porque ninguna función lineal de los píxeles separa
las clases. Una sola capa y un solo filtro, a propósito: lo que se enseña es
que un kernel detecta un rasgo básico.

**`7-Recurrente.html` — la red recurrente más chica, cinco parámetros a mano.**
La red usa el mismo parámetro en cada paso, y el estado es su memoria.

## Lo que comparten

Las cuatro usan el mismo diagrama, los mismos doscientos puntos partidos en 160
de entrenamiento y 40 de prueba, el mismo contador sobre las dos particiones y
el mismo sorteo con semilla, para que todo el grupo abra el mismo tablero y se
pueda hablar del mismo dibujo. Las dos últimas cambian de diagrama y de datos
—una CNN y doscientas imágenes de 6×6—, pero conservan la partición 160/40, el
contador sobre las dos particiones y el sorteo con semilla.

Las tres primeras comparten además el gesto: se hace clic en una conexión y el
deslizador mueve ese parámetro. Abren con pesos al azar y no con la solución, y
tienen un botón para pedirla cuando el alumno ya se hizo la pregunta. La cuarta
no tiene ese gesto ni ese botón —no hay nada que revelar donde el algoritmo va a
llegar solo— y en su lugar tiene la tasa, los tres botones de paso y uno para
volver a los pesos iniciales.

Y las cuatro descansan en la misma promesa, que llamamos el ancla: la línea que
se señala es el parámetro que se mueve. Ante la duda no se selecciona nada, y el
diagrama sale de un arreglo de descriptores que se verifica antes de dibujar
nada. En la tercera el ancla tiene una segunda mitad, porque hay otro gesto: el
nodo que se señala es la activación que cambia. En la cuarta se dice al revés
—la arista que se ilumina es el peso que cambia— y la verificación crece: antes
de montar, el gradiente analítico se compara contra diferencias centradas sobre
los diecisiete parámetros. Si el paso hacia atrás no fuera la derivada de la
pérdida, la página no arranca. En la quinta y la sexta el ancla no tiene gesto
de selección: la ventana, su celda y su nodo son la misma posición y se
iluminan juntas, pero señalar no elige ningún peso.

Toda la aritmética es la nativa de JavaScript, binary64. Las arquitecturas, las
fórmulas, los datos, las escalas y las cifras medidas están en `Explica.md`.

## Licencia

MIT — ver `LICENSE`.
