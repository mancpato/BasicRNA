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

**Las otras tres no caben proyectadas.** La cuarta se dimensionó para entrar
entera en una ventana de 940 px y no usar el scroll en clase. Las tres primeras
no lo hacen, y ahora la diferencia se nota al pasar de una a otra. Es una tarde
de trabajo aplicar el mismo criterio a las tres, y probablemente valga la pena.

## 4. Páginas 5 y 6: una CNN mínima

Decidida en lo esencial; lo abierto está al final. Las cifras salen de un
arnés en Python con 100 semillas y SGD de a una imagen; hay que volver a
medirlas en JavaScript antes de escribirlas en Explica.md.

**Qué red.** Entrada 6×6, un filtro 3×3 sin relleno (mapa 4×4), activación,
máximo global y salida sigmoide σ(v·m + c): doce parámetros. Datos: 200
imágenes (160/40), un segmento de longitud 3 vertical u horizontal, con
intensidad en [0.75, 1] sobre fondo con ruido en [0, 0.25], que no toca el
borde lateral. Sin ese margen, un segmento en la columna 0 sólo pasa bajo una
columna del filtro y ningún filtro único resuelve el problema.

**Por qué pertenece aquí.** El ancla deja de ser una arista por parámetro:
una celda del filtro es el mismo número en las dieciséis posiciones, y al
seleccionarla se encienden dieciséis aristas. Pasar el ratón por una celda
del mapa enciende sus nueve píxeles. Parámetros compartidos y localidad, un
gesto cada uno. Y el perceptrón de la primera página no puede con estos
datos: las tres columnas de un cuadrado 3×3 suman lo mismo que sus tres
filas, así que ninguna función lineal de los píxeles separa las clases
(confirmado por programación lineal sobre todos los segmentos).

**Página 5, a mano.** Clic en una celda del filtro y el deslizador la mueve.
En lugar del mapa de frontera, que ya no existe porque la entrada tiene 36
dimensiones, una galería de miniaturas con el contador, y un lienzo 6×6 donde
el alumno pinta su propio trazo. Dos interruptores: activación (lineal o
ReLU) y agregación (máximo o promedio). Con promedio y lineal la red entera
es lineal en los píxeles y ningún filtro llega a cero errores, por el
argumento del cuadrado: es una cota demostrada, no resultado de búsqueda,
como la de las franjas en la tercera página. La lección es que la no
linealidad tiene que estar en algún lado. Con máximo, cualquier activación
creciente conmuta con él, φ(máx z) = máx φ(z), así que la activación no
cambia lo que la red puede decidir. Ante empate del máximo, frecuente con
trazos pintados en 0/1, se marcan todas las celdas empatadas: ante la duda
no se elige una.

**Página 6, entrena.** El mismo mecanismo de la cuarta página: un paso
animado y tandas de 10 y 100. Agregación fija en máximo. Con máximo global el
ajuste del filtro es −η·δ·P*, con P* el parche 3×3 que ganó y
δ = (ŷ − t)·v·φ′(z*): el parche se desprende de la imagen y se suma o se
resta sobre el filtro. Se exponen η y la activación, nada más.

**La activación es la perilla que enseña.** Con ReLU, 40 de 100 semillas
llegan a cero errores; con lineal, 97 (tanh, 51). En las 60 que se atoran
con ReLU, el 98 % de las imágenes mal clasificadas tienen máx z ≤ 0: la ReLU
apaga justo los ejemplos que podrían corregir el filtro, que casi siempre
quedó como franja descentrada que no alcanza los segmentos junto al borde.
Con lineal, en las mismas semillas, el filtro a veces inventa soluciones que
nadie diseñaría, como dos franjas laterales con hueco al centro, que cubren
los dos bordes. Enlaza con las neuronas muertas de la cuarta página. Hay que
decir en la página que la ReLU sobra aquí porque el máximo ya es la no
linealidad, y que en la segunda página era indispensable, para que el alumno
no se lleve «la ReLU es mala».

**La tasa.** Entre 0.01 y 0.10 el éxito no se mueve (40 de 100 con ReLU) y
sólo cambia la rapidez: mediana de 800 a 200 iteraciones. En 0.3 baja a 31 y
en 1.0 a 10. Cambia cuánto tarda, no adónde llega, mientras no sea grande.

**Lo que no entra.** Momento: con el paso efectivo igualado, η(1−β), no
cambia el éxito (40 y 42 de 100); sin igualarlo, con β = 0.9 baja a 25. No
rescata ninguna semilla atorada, y rompe la identidad visual del ajuste: el
cambio del filtro deja de ser el parche de este paso. Además la cuarta página
ya lo dejó en TalleRNA. Relleno: con ReLU sube el éxito a 94 de 100, pero
arregla lo mismo que el interruptor de activación, dos perillas para una
lección; se cuenta en Explica.md. Número de filtros, stride y tamaños quedan
fijos por construcción.

**Abierto.** La semilla: con ReLU se atora el 60 %, así que la semilla por
omisión decide lo que el alumno ve primero, y un botón de «otra semilla» es
exponer la inicialización, que la cuarta página dejó en TalleRNA. Con
activación lineal, b y c son redundantes (dirección exactamente plana:
b+ε, c−vε): ¿se quita b en ese modo o se deja para mostrar que moverlo no
cambia nada? La partición de prueba repite el problema del punto 3. Los
nombres de archivo.

**Lo que obliga a cambiar en el README.** La sección «Lo que sigue» es
provisional y se funde en el cuerpo cuando existan las páginas. Tres pasajes
se vuelven falsos. La frontera dice que la cuarta página expone «un solo
hiperparámetro»; la sexta expone también la activación. El criterio
sobrevive —se expone sólo lo que la animación vuelve visible, y con ReLU se
ve que a veces no regresa ningún parche—, pero la redacción no. «Lo que
comparten» dice que todas usan el mismo diagrama y los mismos doscientos
puntos: las páginas CNN tienen otro diagrama y doscientas imágenes, aunque la
partición 160/40, el contador y la semilla sí se comparten. Y el ancla deja
de ser una arista por parámetro para ser una familia de dieciséis, que se
verifica como traslaciones unas de otras. La portada y el index.html
necesitan dos miniaturas más.

## Lo que no está pendiente

Que la tercera página se preste a un estudio más profundo —activaciones
aprendidas, búsqueda sobre el espacio de funciones, la conexión con KAN— no es
un pendiente de BasicRNA. Eso vive en la sección de redes especiales, después de
TalleRNA, y estas páginas no tienen que prepararlo más de lo que ya lo hacen.

Tampoco lo es abrir más hiperparámetros en la cuarta página. La frontera del
directorio es que aquí no se configura: momento, tamaño de lote, épocas e
inicialización son de TalleRNA. Si se abren aquí, esta página ya es TalleRNA y
el directorio pierde su límite.
