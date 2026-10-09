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
- **D17**, el lazo como matriz es la undécima (2026-10-09). La página del lazo
  como matriz es la 11, `11-LazoComoMatriz.html`, y va después de la de varias
  neuronas. Es la primera página que nace de una especificación escrita antes de
  construirla, porque Miguel quiso estudiar la teoría primero; Claude la
  construyó en el chat y Miguel la aprobó.
- **D18**, la atención se queda sin número (2026-10-09). La atención ya no es la
  undécima: antes puede venir otra página de recurrencia. `ESPEC-11-Atencion.md`
  conserva su nombre y su contenido, y el número se le fija cuando le toque.
  Deja sin efecto la parte de D12 que la hacía la undécima.
- **D19**, la serie recurrente son cinco páginas (2026-10-09). De la 7 a la 11:
  la neurona en el tiempo, la recurrente a mano, la retropropagación en el
  tiempo, varias neuronas de estado y el lazo como matriz. Con eso queda cerrada;
  D12 la cerraba en cuatro.
- **D20**, la teoría de la undécima vive en `Explica.md` (2026-10-09). La
  especificación previa, `BasicRNNspec.md`, pasa a una sección de la 11 en
  `Explica.md`, con la notación matricial tal como estaba —vectores y matrices
  escritos como arreglos—, y con tres correcciones que la página ya traía: k
  llega a 39 y no a 40, el empuje va de 0 a 2 y empieza en 0.2, y donde la
  especificación decía «verificar al construir» van las cifras medidas. El
  original se mueve a `../BasicRNA-trabajo/` y no entra al repositorio.
- **D21**, el `README.md` es para colegas (2026-10-09). Dice la idea del
  proyecto, cómo abrir las páginas y qué enseña cada una, y nada del proceso de
  trabajo: ni decisiones, ni pendientes, ni cifras, ni planes. `CLAUDE.md`,
  `DECISIONES.md`, `TODO.md` y los `ESPEC-*.md` quedan nombrados en una línea
  como lo que son, documentos del proceso.
- **D22**, `doc/` guarda el apoyo didáctico (2026-10-09). La carpeta `doc/` es
  para los documentos con que Miguel estudia: teoría explícita, notas y PDF.
  Es, junto con `Explica.md`, una excepción declarada a la lista cerrada de
  documentos del `~/.claude/CLAUDE.md`, y así queda dicho en el `CLAUDE.md` del
  proyecto. Lo de desarrollo —`README.md`, `CLAUDE.md`, `DECISIONES.md`,
  `TODO.md`, `Explica.md` y los `ESPEC-*.md`— se queda en la raíz.
- **D23**, la teoría de la undécima vive en `doc/LazoComoMatriz.md` (2026-10-09).
  Reemplaza la parte de D20 que la mandaba a `Explica.md`: ahí queda una línea
  que apunta a `doc/`. El documento es la especificación previa sin su sección
  «Lo que muestra la página», que es desarrollo y vive en la cabecera de la
  página, y con las correcciones que ya traía la sección de `Explica.md`.
- **D24**, la atención es la 12 (2026-10-09). `ESPEC-12-Atencion.md` y, cuando
  exista, `12-Atencion.html`. Reemplaza a D18, que la dejaba sin número. Será
  una red recurrente pequeña a la que se agrega un mecanismo de atención
  básico, y va después del lazo como matriz.
- **D25**, el tope del `README.md` son 40 líneas (2026-10-09). No es una
  decisión nueva: venía de antes y vivía en los pendientes de mantenimiento de
  `TODO.md`, que se borraron al reescribirlo, así que quedó sin registro en el
  disco. El `~/.claude/CLAUDE.md` no fija tope al README; éste es el del
  proyecto.
- **D26**, el `README.md` no lleva tope de líneas (2026-10-09). Basta con que sea breve. Reemplaza a D25.
- **D27**, `Explica.md` documenta el desarrollo de las páginas (2026-10-09). Las arquitecturas, los datos y las cifras medidas, no la teoría; la teoría va en `doc/`.
