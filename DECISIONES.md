# Decisiones de BasicRNA

Una línea por decisión, con su identificador. No se poda: lo que entra se queda,
para que una sesión futura no vuelva a discutirlo. Lo que el código ya dice por
sí mismo —colores, etiquetas, tamaños— no entra aquí.

- **D1**, el orden de la serie (2026-10-05). La página de la neurona en el
  tiempo entra como séptima y recorre a las que seguían: la recurrente a mano
  pasa a octava, la retropropagación en el tiempo a novena, y la atención, que
  estaba planeada como novena, a décima, con su especificación renombrada en
  consecuencia. (El número de la atención volvió a correrse en D12.)
- **D2**, los nombres de archivo (2026-10-05). El archivo nuevo se llama
  `7-NeuronaEnElTiempo.html`; los demás conservan su nombre con el número nuevo,
  `8-Recurrente.html` y `9-BackpropTiempo.html`, y los cambios se hacen con
  `git mv` para que git conserve la historia de cada archivo.
- **D3**, la forma de la séptima (2026-10-05). La neurona va fija arriba y el
  registro crece abajo, y no la neurona avanzando por la tira y dejando copias
  detrás; el plegado es vertical, con la entrada arriba como cada columna del
  desenrollado, y no de izquierda a derecha como las páginas 1 a 6; el alumno
  pinta los píxeles y elige la longitud, 7 o 9; y la página lleva neurona de
  salida, que con recuerda pregunta por la mayoría y con olvida por el último
  píxel. Las propuso Claude y Miguel las aprobó al revisar la página construida.
- **D4**, los dos dibujos de la octava (2026-10-05). La octava conserva el
  desenrollado, porque es lo que enseña que todas las copias de un peso son el
  mismo número, y la red dibujada una vez va arriba a la izquierda, en el mismo
  renglón que el deslizador, que es donde vive el gesto de mover a mano.
- **D5**, la selección y los rótulos de la octava (2026-10-05). Un peso se
  selecciona desde cualquiera de los dos dibujos y el resaltado cubre su línea
  de arriba y todas sus copias de abajo; junto a cada línea de arriba va escrito
  el valor de su peso, para leer los cinco sin moverlos; y la página dice
  «momento» donde decía «paso», como la séptima.
- **D6**, lo que se encogió para que cupiera (2026-10-05). El deslizador pasó de
  640 a 380 px de largo y el panel de 266 a 198 px de alto, para hacerle sitio a
  la red dibujada una vez sin salirse de 1196 × 940.
- **D7**, dónde cae la suma en la novena (2026-10-05). La red dibujada una vez
  es el lugar donde cae la suma: en el último tiempo de «Un paso» las copias
  piden, lo que piden sube hacia la única línea de su peso, y ahí cae la suma.
  No lleva δ, ni factores, ni gesto, porque la cadena ocurre abajo. Es el camino
  inverso del de la octava (D4): allá un peso mueve sus copias, aquí las copias
  empujan al peso.
- **D8**, el primer renglón de la novena (2026-10-05). La red dibujada una vez
  va arriba a la izquierda de la caja del dibujo, y los contadores y la curva de
  pérdida a su derecha; desaparece la franja de instrumentos a todo lo ancho y
  la curva se angosta a 396 px.
- **D9**, la escala común del pulso (2026-10-05). Lo que pide cada copia y las
  cinco sumas se miden contra una sola escala, el mayor de los dos en ese paso,
  para que lo pedido y lo recibido se puedan comparar a la vista.
- **D10**, momento y paso (2026-10-05). «Momento» nombra un punto de la tira, y
  «Un paso» sigue nombrando un paso de entrenamiento, que es una tira completa.
- **D11**, sin página que entrene la atención (2026-10-05). No habrá una página
  que entrene la red de atención: con cuatro parámetros a mano la décima se
  sostiene sola. Por eso el contraste del camino del error —en la recurrente la
  señal que llega al primer elemento pasa por una cadena de multiplicaciones, y
  en la atención por un solo peso— se dice en clase y no se muestra en ninguna
  página, y el gozne que la novena anuncia pasa a ser el de la memoria: resumir
  en un número contra buscar entre todo lo anterior.
- **D12**, la serie recurrente se cierra con varias neuronas (2026-10-06). La
  página de varias neuronas recurrentes es la décima, con las dos tareas —la
  paridad y el residuo entre tres— en una sola página y un selector; la atención
  pasa a ser la undécima, con `ESPEC-11-Atencion.md` y `11-Atencion.html`. Con
  ella la memoria deja de ser un número y pasa a ser un vector, que es el mismo
  paso que de la primera página a la segunda.
- **D13**, el dibujo por capas (2026-10-06). Con varias neuronas la red se
  dibuja a la manera de Elman: una columna «de antes» con una copia de cada
  neurona, de la que salen las líneas de regreso. Rompe con el lazo de la
  séptima a la novena a propósito, porque con tres neuronas las n² flechas de
  regreso dibujadas como lazos son una maraña.
- **D14**, el interruptor del panel (2026-10-06). Un interruptor muestra en el
  mismo lugar el desenrollado o el estado como un punto, un cuadrado con dos
  neuronas y un cubo con tres. En el desenrollado no se dibujan los sesgos de
  estado; en el estado, los nombres de los sitios sólo aparecen con la solución
  cargada.
- **D15**, qué solución carga cada tarea (2026-10-06). La del residuo entre tres
  es la red hallada por búsqueda y redondeada a múltiplos de 0.5, porque cabe en
  el tope 6 de la serie mientras que la versión tanh exacta de la máquina de
  umbral pide un peso de 7; en clase se explica con esa máquina de umbral, que
  la red realiza. La de la paridad sí es la escrita.
- **D16**, la copia va de capa a capa (2026-10-06). En la décima, la copia de un
  momento al siguiente une las dos columnas enteras: un marco punteado rodea la
  columna de estado, otro la columna «de antes», y una sola flecha los une.
  Antes salía de h1 sola y parecía que sólo h1 se copiaba. Al pie del dibujo, un
  rótulo dice que con una sola neurona la casilla «de antes» y su línea son el
  lazo de la octava, con lo que las dos gramáticas de dibujo de la serie
  recurrente quedan unidas en pantalla y no sólo en clase.
