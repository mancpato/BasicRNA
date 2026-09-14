# BasicRNA

Tres páginas sueltas para las primeras clases sobre redes neuronales. Cada una
es un archivo HTML único, sin dependencias y sin build: se abre con doble clic y
dibuja con canvas 2D.

Son deliberadamente pobres en perillas. No hay tasa de aprendizaje, ni número de
épocas, ni tamaño de lote, ni inicialización, ni ningún otro hiperparámetro: no
se entrena nada. El alumno mueve los parámetros a mano, uno a la vez, y ve qué
le pasa a la frontera. Los hiperparámetros llegan después, con TalleRNA y las
demás herramientas de las que este directorio es hermano.

## Las tres páginas, en orden

**`explorador-neurona.html` — Una neurona: los tres parámetros del perceptrón.**
Una sola neurona con salida sigmoide y tres parámetros. Cada uno hace algo que
se puede decir en una frase: los dos pesos giran la recta, el sesgo la desplaza.
El alumno llega a cero errores a mano sin demasiado esfuerzo, y sale sabiendo lo
que se siente entender un parámetro. Trae los pesos entrenados
(3.0041, 2.5568, 0.0366) detrás de un botón, para después de haberlo intentado.

**`explorador-parametros.html` — red 2→4→1, diecisiete parámetros.**
La misma mecánica con una capa oculta de cuatro neuronas, y un interruptor que
cambia la activación de toda la capa entre ReLU y tanh. Aquí la intuición ya no
alcanza: mover un peso cambia el mapa de un modo que nadie anticipa, y ése es el
punto de tenerla. Los datos son un disco contra el anillo que lo rodea, que
ninguna recta separa, así que la neurona sola de la página anterior no podría
con ellos. Las dos redes horneadas llegan a cero mal clasificados en las dos
particiones.

**`explorador-activaciones.html` — una función por neurona.**
La misma red 2→4→1, pero la activación se elige neurona por neurona: clic en el
nodo y un menú de seis funciones —lineal, escalón, ReLU, tanh, gaussiana y
seno—, cada una con su dibujo dentro del círculo. Las seis no son un catálogo de
fórmulas sino una casilla cada una, y la casilla se ve en el mapa: ninguna
frontera propia, una recta con salto, una recta y un doblez, una recta
difuminada, una banda, bandas repetidas. Trae tres conjuntos de puntos elegidos
midiendo cuántas neuronas necesita cada función para llegar a cero errores, y el
tercero es el que decide: con franjas, una sola neurona seno lo resuelve y
cuatro ReLU no llegan nunca.

## Lo que comparten

Las tres usan el mismo diagrama y el mismo gesto: se hace clic en una conexión y
el deslizador mueve ese parámetro. Todas cuentan mal clasificados sobre 160
puntos de entrenamiento y 40 de prueba, abren con pesos al azar y no con la
solución, y tienen un botón para pedirla cuando el alumno ya se hizo la
pregunta.

Y las tres descansan en la misma promesa, que llamamos el ancla: la línea que se
señala es el parámetro que se mueve. Ante la duda no se selecciona nada, y el
diagrama sale de un arreglo de descriptores que se verifica antes de dibujar
nada, no de un cálculo hecho sobre la marcha. En la tercera página el ancla
tiene una segunda mitad, porque hay otro gesto: el nodo que se señala es la
activación que cambia.

El grosor de cada línea se mide contra el tope del deslizador y no contra el
mayor parámetro de la red, con una raíz para no perder resolución en los valores
chicos. Es la única forma de que mover un parámetro mueva una sola línea, que es
lo que el ancla promete.

Toda la aritmética es la nativa de JavaScript, binary64.
