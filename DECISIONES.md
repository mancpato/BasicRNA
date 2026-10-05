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
