# BasicRNA

Cuatro páginas sueltas para las primeras clases sobre redes neuronales. Cada una
es un archivo HTML único, sin dependencias y sin build: se abre con doble clic y
dibuja con canvas 2D.

Son deliberadamente pobres en perillas. La frontera de este directorio no es que
aquí no se entrene, sino que aquí no se configura: las tres primeras páginas no
entrenan nada —el alumno mueve los parámetros a mano, uno a la vez, y ve qué le
pasa a la frontera— y la cuarta entrena con lo mínimo expuesto. De todos los
hiperparámetros se abre uno solo, la tasa de aprendizaje, que es el único cuyo
efecto la animación vuelve visible. Momento, tamaño de lote, número de épocas e
inicialización se quedan en TalleRNA y las demás herramientas de las que este
directorio es hermano. Si se abrieran aquí, la cuarta página ya sería TalleRNA.

## Las cuatro páginas, en orden

**`1-Perceptron.html` — Una neurona: los tres parámetros del perceptrón.**
Una sola neurona con salida sigmoide y tres parámetros. Cada uno hace algo que
se puede decir en una frase: los dos pesos giran la recta, el sesgo la desplaza.
El alumno llega a cero errores a mano sin demasiado esfuerzo, y sale sabiendo lo
que se siente entender un parámetro. Trae los pesos entrenados
(3.0041, 2.5568, 0.0366) detrás de un botón, para después de haberlo intentado.

**`2-EditParam.html` — red 2→4→1, diecisiete parámetros.**
La misma mecánica con una capa oculta de cuatro neuronas, y un interruptor que
cambia la activación de toda la capa entre ReLU y tanh. Aquí la intuición ya no
alcanza: mover un peso cambia el mapa de un modo que nadie anticipa, y ése es el
punto de tenerla. Los datos son un disco contra el anillo que lo rodea, que
ninguna recta separa, así que la neurona sola de la página anterior no podría
con ellos. Las dos redes horneadas llegan a cero mal clasificados en las dos
particiones.

**`3-EditActivFun.html` — una función por neurona.**
La misma red 2→4→1, pero la activación se elige neurona por neurona: clic en el
nodo y un menú de seis funciones —lineal, escalón, ReLU, tanh, gaussiana y
seno—, cada una con su dibujo dentro del círculo. Las seis no son un catálogo de
fórmulas sino una casilla cada una, y la casilla se ve en el mapa: ninguna
frontera propia, una recta con salto, una recta y un doblez, una recta
difuminada, una banda, bandas repetidas. Trae tres conjuntos de puntos elegidos
midiendo cuántas neuronas necesita cada función para llegar a cero errores, y el
tercero es el que decide: con franjas, una sola neurona seno lo resuelve y
cuatro ReLU no llegan nunca.

**`4-Backprop.html` — quién acomoda los diecisiete.**
La red de la segunda página y sus mismos doscientos puntos, pero aquí no se
mueve ningún parámetro a mano: se ve cómo los mueve el algoritmo. Se sortea un
punto, se ilumina en el mapa, sus dos coordenadas entran, activan la capa
oculta, llegan a la salida y producen una decisión; se compara con la clase
verdadera y esa diferencia regresa por la red y ajusta los diecisiete. Un paso
de descenso por gradiente estocástico con una sola muestra, sin momento y sin
lote.

El problema que arrastra es que ver un paso y ver que la red aprende están
separados por tres órdenes de magnitud. La salida no son dos modos distintos
sino un solo mecanismo con tres botones: «Un paso» lo anima completo, en siete
tiempos y cuatro segundos y medio, y +10 y +100 repiten exactamente ese mismo
paso con la animación apagada. Medido con la inicialización y la semilla que
trae el archivo, con η = 0.05: ReLU llega a cero mal clasificados en las dos
particiones en la iteración 1413 y tanh en la 2401. Son catorce y veinticuatro
apretones de +100, y esa distancia entre el paso que se entiende y la cantidad
de pasos que hacen falta no es un defecto de la página: es la materia.

Dos cosas se dibujan distinto a propósito, porque son distintas. Hacia atrás
circula δ, la señal de error de cada neurona, que depende del peso; lo que
cambia cada arista es −η·δ_j·a_i, que además depende de la activación que entra
por ella. Si se iluminaran con el mismo recurso visual el alumno se llevaría la
idea de que el error corre por el cable y de paso lo modifica. Por eso δ vive en
el nodo, como un anillo cuyo grosor es |δ|, y el ajuste vive en la arista, como
un pulso punteado; y el signo del ajuste lleva dos colores propios, violeta y
ocre, porque el azul y el rojo ya significan el signo del peso.

La ganancia sale gratis: retropropagar multiplica por φ′(z), y con ReLU eso vale
0 o 1, así que una neurona con z ≤ 0 queda muerta en el paso hacia atrás, no le
llega nada y sus cinco aristas no cambian. Se ve solo, y la página lo rotula.
Enlaza con la tercera: el escalón tiene derivada cero en todas partes, que es
por qué TalleRNA le pone la etiqueta ∇=0, y aquí se vería como que no regresa
nada por ninguna arista, nunca.

## Lo que comparten

Las cuatro usan el mismo diagrama, los mismos doscientos puntos partidos en 160
de entrenamiento y 40 de prueba, el mismo contador de mal clasificados sobre las
dos particiones, y el mismo sorteo con semilla, para que todo el grupo abra el
mismo tablero y se pueda hablar del mismo dibujo.

Las tres primeras comparten además el gesto: se hace clic en una conexión y el
deslizador mueve ese parámetro. Abren con pesos al azar y no con la solución, y
tienen un botón para pedirla cuando el alumno ya se hizo la pregunta. La cuarta
no tiene ese gesto ni ese botón —no hay nada que revelar donde el algoritmo va a
llegar solo—, y en su lugar tiene la tasa, los tres botones de paso y uno para
volver a los pesos iniciales.

Y las cuatro descansan en la misma promesa, que llamamos el ancla: la línea que
se señala es el parámetro que se mueve. Ante la duda no se selecciona nada, y el
diagrama sale de un arreglo de descriptores que se verifica antes de dibujar
nada, no de un cálculo hecho sobre la marcha. En la tercera página el ancla
tiene una segunda mitad, porque hay otro gesto: el nodo que se señala es la
activación que cambia. En la cuarta la promesa se dice al revés y la verificación
crece: la arista que se ilumina es el peso que cambia, y antes de montar nada el
gradiente analítico se compara contra diferencias centradas sobre los diecisiete
parámetros, en las dos activaciones. Si el paso hacia atrás no fuera la derivada
de la pérdida, la página no arranca. Una animación de retropropagación que
retropropague otra cosa sería peor que no tenerla.

El grosor de cada línea se mide contra un tope fijo —el del deslizador donde lo
hay— y no contra el mayor parámetro de la red, con una raíz para no perder
resolución en los valores chicos. Es la única forma de que mover un parámetro
mueva una sola línea, que es lo que el ancla promete, y de que una línea de
cierto grosor quiera decir lo mismo en las cuatro páginas.

Toda la aritmética es la nativa de JavaScript, binary64.
