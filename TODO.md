# Pendientes de BasicRNA

Nada de esto está decidido salvo donde se diga. Son huecos y planes anotados
después de terminar las tres páginas, para no volver a razonarlos desde cero.
Los dos últimos esperan los comentarios de los profesores del DASC.

## 1. Cuarta página: backpropagation paso a paso

Decidida en lo esencial; el código va en una sesión de trabajo aparte. Entrena,
a diferencia de las otras tres, y vive en este mismo directorio.

**Qué hace.** Se ilumina un punto de entrenamiento tomado al azar, sus dos
coordenadas entran a la red, se ve cómo avanzan y activan las neuronas ocultas,
se propagan a la salida y se toma una decisión. Se evalúa el error y se usa para
ajustar pesos, iluminando aristas en reversa. Iteración tras iteración las
líneas cambian de grosor.

**Los dos relojes, y cómo se resuelven.** Ver un paso en detalle y ver que la
red aprende están separados por tres órdenes de magnitud: una actualización con
tasa usual mueve un peso en centésimas, invisible en el grosor. La solución
acordada no son dos modos sino un paso a paso y botones de +10 y +50 que repiten
el mismo paso con la animación apagada. Es un solo mecanismo, que es lo que hay
que poder explicar.

Dos detalles que ese diseño arrastra. Al terminar una tanda conviene apagar el
resalte en vez de dejarlo en lo que tocó el último punto, porque sería señalar
un caso particular como si resumiera los cincuenta. Y el salto de +1 a +10 va a
decepcionar con una tasa realista: o el botón grande es +100, o la tasa de esta
página es más alta que la de entrenar en serio, y entonces hay que decirlo en la
página para que el alumno no lo generalice.

**Qué viaja de regreso, y dónde se puede mentir.** Hacia atrás circula δ, la
señal de error de cada neurona, que depende del peso. Lo que cambia cada arista
es δ_j·a_i, que depende de la activación de entrada. Son dos cantidades
distintas sobre la misma arista, y si se iluminan con el mismo recurso visual el
alumno se lleva la idea de que el error corre por el cable y de paso lo modifica.
Propuesta: δ en el nodo, con halo en el círculo; el ajuste en la arista, con
pulso o cambio de grosor. El signo del ajuste no puede reusar el rojo y el azul,
que ya significan el signo del peso.

**La ganancia que sale gratis.** Backpropagation multiplica por φ'(z). Con ReLU
eso vale 0 o 1, así que las neuronas con z negativo quedan muertas en el paso
hacia atrás y no les llega nada, y eso se ve. Enlaza con la tercera página: el
escalón tiene derivada cero en todas partes, que es por qué TalleRNA le pone la
etiqueta ∇=0, y aquí se vería como que no regresa nada por ninguna arista.

**Dos cosas menores.** El punto que se ilumina sale del sorteador con semilla,
como todo lo demás, para poder repetir la misma secuencia en clase. Y el ancla
crece otra vez: la arista que se ilumina es el peso que cambia.

**La frontera con TalleRNA.** Exponer sólo la tasa de aprendizaje, que es la
única cuyo efecto la animación vuelve visible. Momento, tamaño de lote e
inicialización se quedan en TalleRNA; si se abren aquí, esta página ya es
TalleRNA y el directorio pierde su límite.

**Lo que obliga a cambiar en el README.** El primer párrafo dice hoy que aquí no
se entrena nada. Cuando esta página exista, la frontera deja de ser «no se
entrena» y pasa a ser «no se configura»: las tres primeras no entrenan y la
cuarta entrena con lo mínimo expuesto. Es una frontera más honesta y además
explica por qué esta página pertenece aquí y no a TalleRNA.

## 2. No se ve nunca una cantidad continua que baje

Prerrequisito de la cuarta página, no sólo mejora de la primera.

**El hueco.** Las tres páginas miden con el contador de mal clasificados, que es
entero y plano. El alumno arrastra un peso medio recorrido, la cifra no se
inmuta, y después salta de golpe. De ahí pasa a TalleRNA, donde se le dice que
hay un algoritmo que sigue una pendiente, sin haber visto nunca una pendiente.
Lo que sí vio fue un indicador que se queda quieto mientras él se mueve, que es
la lección contraria.

**Por qué la cuarta página lo necesita.** Si se actualiza de a un punto, el
contador entero va a brincar y a veces a empeorar. Sin una curva continua al
lado, el alumno no tiene cómo ver que aun así se está bajando.

**Dónde metería el arreglo.** En `1-Perceptron.html`, no en una página
nueva. Es donde el espacio tiene tres parámetros y todo se puede decir en una
frase, y donde el alumno todavía tiene atención libre. En las otras dos
competiría con lo que ya están enseñando.

**Qué sería.** Al lado del contador, el error suave —la log-verosimilitud, que
es exactamente lo que minimiza el descenso por gradiente— y, del parámetro
seleccionado, su curva a lo largo de todo el recorrido del deslizador con la
posición actual marcada. El alumno arrastra, ve el contador estancado y la curva
bajando, y entiende que hay un fondo hacia el que conviene ir. Es un corte
unidimensional de la función que TalleRNA va a bajar.

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

## 3. Los cuarenta puntos de prueba no trabajan

**El hueco.** Las tres páginas parten los 200 puntos en 160 de entrenamiento y
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
todo. Sobreajustar a mano con diecisiete parámetros es posible, pero hay que
medir si se alcanza moviendo parámetros de uno en uno o si sólo se llega
entrenando.

La cuarta página cambia el cálculo, porque ahí sí se entrena y el sobreajuste
puede aparecer solo. Conviene decidir esto después de tenerla, no antes.

**Cuál prefiero.** Ninguna de las dos sin medir antes. Lo que sí es claro es que
el estado actual es el peor de los tres: paga el costo visual de la distinción
sin cobrar el beneficio.

## Lo que no está pendiente

Que la tercera página se preste a un estudio más profundo —activaciones
aprendidas, búsqueda sobre el espacio de funciones, la conexión con KAN— no es
un pendiente de BasicRNA. Eso vive en la sección de redes especiales, después de
TalleRNA, y estas páginas no tienen que prepararlo más de lo que ya lo hacen.
