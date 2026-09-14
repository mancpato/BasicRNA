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

## Lo que no está pendiente

Que la tercera página se preste a un estudio más profundo —activaciones
aprendidas, búsqueda sobre el espacio de funciones, la conexión con KAN— no es
un pendiente de BasicRNA. Eso vive en la sección de redes especiales, después de
TalleRNA, y estas páginas no tienen que prepararlo más de lo que ya lo hacen.

Tampoco lo es abrir más hiperparámetros en la cuarta página. La frontera del
directorio es que aquí no se configura: momento, tamaño de lote, épocas e
inicialización son de TalleRNA. Si se abren aquí, esta página ya es TalleRNA y
el directorio pierde su límite.
