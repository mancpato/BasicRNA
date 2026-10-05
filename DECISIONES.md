# Decisiones de BasicRNA

Una línea por decisión, con su identificador. No se poda: lo que entra se queda,
para que una sesión futura no vuelva a discutirlo. Lo que el código ya dice por
sí mismo —colores, etiquetas, tamaños— no entra aquí.

- **D1**, el orden de la serie (2026-10-05). La página de la neurona en el
  tiempo entra como séptima y recorre a las que seguían: la recurrente a mano
  pasa a octava, la retropropagación en el tiempo a novena, y la atención, que
  estaba planeada como novena, a décima, con su especificación renombrada a
  `ESPEC-10-Atencion.md`.
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
