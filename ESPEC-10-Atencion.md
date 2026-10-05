# Especificación: la página de atención

Estado: **propuesta, antes de construir.** El número 10 está decidido; la 9 es
la retropropagación en el tiempo de la red de la página 8. Archivo previsto:
`10-Atencion.html`.

Las cifras de este documento son mediciones hechas en JavaScript, binary64,
sobre todas las tiras de 4 y de 6 flechas. Lo que todavía no se ha medido está
marcado como **abierto**.

## 1. Por qué esta página

En todas las páginas anteriores el grosor de una línea es un **parámetro**: un
número fijo que el alumno mueve o que el entrenamiento ajusta. En la atención
los pesos de las líneas **se calculan a partir de la entrada** y cambian con
cada tira: la red decide a quién mirar según lo que ve. Ésa es la idea de la
página, y por eso rompe a propósito la gramática visual de la serie.

El contraste con la página 8 se dice en una frase: la red recurrente recuerda
**resumiendo** todo en un número; la atención recuerda **buscando** entre todo
lo anterior.

Dos ideas, cada una con su gesto:

- **La mirada se calcula.** Se cambia una flecha de la tira y los pesos de
  atención cambian sin tocar ningún parámetro.
- **La mirada no sabe el orden.** «Barajar» revuelve las flechas anteriores y
  ŷ no cambia. Por eso un transformer necesita codificación de posición.

Queda fuera el transformer completo: la red de la página 2 en cada posición,
las conexiones residuales, la normalización, las varias cabezas y la
codificación de posición. En clase se dice que un transformer apila muchas
veces esta pieza junto con la red de la página 2.

## 2. La red

Los símbolos son cuatro flechas, y el vector de cada una es literalmente su
dirección en el círculo unitario, e(θ) = (cos θ, sin θ):

    →  0°      ↑  90°      ←  180°      ↓  270°

La última flecha de la tira, x_T, **pregunta**; las anteriores, x_1..x_(T−1),
**responden**. Los giros son antihorarios.

    q    = (cos(θ_T + φ), sin(θ_T + φ))       lo que busca
    s_j  = β · q·e_j = β · cos(θ_j − θ_T − φ)  el puntaje de cada anterior
    α_j  = softmax(s)_j                        el peso de atención
    o    = Σ α_j · e_j                         lo que encuentra
    r    = q·o                                 cuánto se parece a lo buscado
    ŷ    = σ(a·r + c)

Cuatro parámetros, cada uno en una frase:

| θ | nombre | qué hace |
|---|---|---|
| θ[0] | φ | hacia dónde mira: gira lo que se busca |
| θ[1] | β | qué tan fijamente mira: con β = 0 promedia, con β grande elige |
| θ[2] | a | peso de la salida, el de siempre |
| θ[3] | c | sesgo de la salida, el de siempre |

**Simplificaciones, dichas en la página y en clase:**

1. Sólo la última flecha pregunta. En un transformer pregunta cada posición.
2. Las proyecciones Q, K y V no se aprenden. El giro φ hace el papel de
   W_Q·W_Kᵀ en su versión 2×2, y V es la identidad.
3. La salida compara lo encontrado con lo buscado (r = q·o), en lugar de la
   lectura lineal estándar.
4. No hay codificación de posición. La página enseña por qué haría falta.

## 3. Las tres tareas

Como en la página 8: tres preguntas, y un solo parámetro las distingue.

| tarea | pregunta de la salida | φ de la solución |
|---|---|---|
| La misma | ¿Apareció antes la misma flecha? | 0° |
| La girada | ¿Apareció antes la flecha girada 90°? | 90° |
| La opuesta | ¿Apareció antes la flecha opuesta? | 180° |

## 4. Los datos

**Entrenamiento:** las 256 tiras de 4 flechas, **todas**, sin sorteo. El
contador de entrenamiento es el error verdadero. Por simetría de giro, las tres
tareas tienen el mismo balance: 148 de 256 tienen la flecha buscada (57.8 %).

**Prueba:** 40 tiras **distintas** de 6 flechas, sorteadas con splitmix32. Son
más largas por la misma razón que en la página 8: miden algo que el
entrenamiento no mide. De las 4096 tiras de 6, 3124 tienen la buscada (76.3 %).
**Abierto:** la semilla, y si se muestrea simple o estratificado. Una sola
muestra no puede quedar balanceada en las tres tareas a la vez.

## 5. Lo que ya se midió

| | 4 flechas (256) | 6 flechas (4096) |
|---|---|---|
| tienen la buscada | 148 (57.8 %) | 3124 (76.3 %) |
| errores con β = 0, mejor umbral | 36 | 593 |
| β mínimo sin errores, mejor umbral | 0.35 | 0.70 |
| β mínimo sin errores, umbral 0.5 | 0.90 | 1.39 |

**El promedio no alcanza.** Con β = 0 la atención es un promedio, y una flecha
buscada se cancela con una opuesta. Hace falta afilar la mirada.

**Más larga la tira, más aguda la mirada.** Con umbral 0.5 (a = 6, c = −3), los
dos β mínimos tienen forma cerrada:

- **Con 4 flechas,** el peor caso es la buscada contra dos opuestas:
  e^(2β) = 6, así que β = ½ ln 6 ≈ 0.896.
- **Con 6,** es la buscada contra cuatro perpendiculares: e^β = 4, así que
  β = ln 4 ≈ 1.386.

Con β entre 0.90 y 1.38 el entrenamiento da cero y la prueba no: con β = 1.0
hay 0 errores de 256 y 1620 de 4096. Como el 40 % de las tiras de 6 fallan,
cualquier muestra de 40 lo detecta. Es la misma lección de la página 8 con otro
mecanismo.

**φ elige la tarea.** Con β = 5 y tiras de 6, cada tarea tiene cero errores con
su φ, y entre 938 y 972 de 4096 con los otros dos.

**Barajar.** Sumando token por token en binary64, la mayor diferencia de r
entre una tira y su versión barajada es 4.4×10⁻¹⁶: la invariancia vale hasta el
redondeo, porque cambia el orden de la suma. Si se sumara agrupando por
símbolo, sería exacta, pero escondería el mecanismo. **Decisión:** sumar token
por token y verificar con tolerancia 1e−12. Para los colegas, el detalle vale
la pena mencionarlo.

## 6. Las soluciones

Escritas, no entrenadas, como el seno de la página 3 y las de la página 8:
β = 6, a = 6, c = −3, y φ = 0°, 90° o 180° según la tarea. Dan cero errores en
todas las tiras de 4 y de 6, con confianza mínima 0.95 en la clase correcta.
Antes de montar se verifica que dan cero y cero en las dos particiones.

## 7. Lo que se ve

Debe caber entera en 1196 × 940 sin desplazamiento, como las páginas 4, 5 y 7.

- **La tira**, que es también el lienzo: flechas en cuadros, con la última
  marcada como la que pregunta. Un clic en una flecha la gira 90°, como el clic
  enciende o apaga un píxel en las páginas 5 y 7.
- **Las líneas de atención**, de la que pregunta a cada anterior. Su grosor es
  α_j contra un tope fijo de 1, que es absoluto, porque los pesos suman 1.
- **El círculo**, que es el dibujo nuevo de la página. En él aparecen:
  - las flechas anteriores como puntos sobre el círculo;
  - lo buscado, q, como la flecha de la que pregunta girada φ, con el arco φ
    dibujado;
  - lo encontrado, o, como un punto **dentro** del círculo, en el centro de
    masa de las anteriores pesadas por atención. Con β chico se queda cerca del
    centro; con β grande salta a la flecha buscada;
  - la proyección r de o sobre q, y la raya del umbral, perpendicular a q.
- **La salida pregunta**, como en las páginas 5 y 7: «¿Apareció antes la misma
  flecha? Sí», con ŷ a dos decimales y la neurona del color de lo que decide.
- **El contador**: mal clasificadas de 256 y de 40.
- **Abierto:** mapa, galería o ambos. El mapa pondría el punto o de cada tira
  en el marco de su q, con la coordenada q·o y la perpendicular. La frontera
  quedaría como una recta vertical, y volvería el mapa de las páginas 1 a 4. La
  galería de la página 8 sirve para cargar tiras y ver cuáles fallan.

## 8. Gestos y controles

- **La barra.** El selector de tarea, y la ranura con la consigna que, al
  segundo parámetro tocado, da lugar a «Mostrar una solución», «Valores al
  azar» y «Reiniciar». Cambiar de tarea no toca θ, como en las páginas 3 y 7.
- **«Barajar las anteriores»**, junto a la tira.
- **φ.** Propuesta: arrastrar la flecha q alrededor del círculo, que es el gesto
  más directo; además, el mando de siempre. **Abierto:** si el deslizador va en
  grados, de −180° a 180° con paso de 1°.
- **β, con el mando.** **Abierto:** su recorrido (propuesta: de 0 a 12) y si se
  dibuja algo más que su efecto en las líneas de atención.
- **a y c**, como líneas a la salida con el grosor contra el tope 6, igual que
  en todas las páginas.
- **Abierto:** el sorteo inicial. La página 8 midió que un sorteo en todo el
  recorrido abre con la salida pegada; aquí hay que medirlo otra vez.

## 9. Gramática visual y ancla

La página agrega una regla a la serie: **lo que se calcula no se selecciona.**
Las líneas de atención no son parámetros, así que un clic sobre ellas no hace
nada, y no llevan azul ni rojo.

El presupuesto de color queda así:

| color | significado |
|---|---|
| azul / rojo | signo de a y de c; nada más |
| gris pizarra | los pesos de atención: lo que se calcula al pasar hacia adelante, como en la página 4 |
| azul / naranja | las dos clases |
| amarillo | el sesgo c |
| violeta / gris | seleccionado y bajo el ratón, como en las páginas 2 y 7 |

El ancla tiene dos promesas:

1. **La línea que se señala es el parámetro que se mueve**, que aquí sólo vale
   para a y c. φ es el ángulo de q y β va en el mando.
2. **Las líneas de atención son los α_j que usa la salida.** El grosor lee el
   mismo arreglo con que se calcula o, sin copias.

## 10. Verificaciones antes de montar

Si alguna falla, no se crea ningún lienzo:

- Las tres soluciones dan cero mal clasificadas en las dos particiones.
- Los α_j suman 1 en todas las tiras, con tolerancia 1e−12.
- Con β = 0, o es exactamente el promedio de las anteriores.
- Barajar las anteriores deja r igual en todas las tiras, con tolerancia 1e−12.
- Girar φ 360° deja todo igual.
- Los descriptores de las líneas de a y c, y los de las líneas de atención,
  apuntan a lo que dicen: a θ[2] y θ[3] las primeras, al α_j de su posición j
  las segundas.

## 11. Guion de clase, en corto

1. **Desde la página 8:** la red recurrente guarda todo en un número. ¿Y si
   pudiera volver a mirar lo anterior?
2. **El promedio:** φ = 0 y β = 0. Falla, y se ve por qué con una tira donde la
   buscada y su opuesta se cancelan.
3. **Afilar:** subir β. Las líneas de atención se concentran, o salta hacia el
   borde del círculo y el contador llega a cero.
4. **Otra tarea:** «La girada». Girar φ 90° y el contador vuelve a cero sin
   tocar nada más.
5. **La prueba:** con β = 1.0 las tiras de 4 salen perfectas y las de 6 no.
   Una tira más larga exige una mirada más aguda.
6. **Barajar:** ŷ no cambia. Ésta es la razón de la codificación de posición.
7. **Cierre:** un transformer es esto con Q, K y V aprendidas, muchas cabezas,
   todas las posiciones preguntando, y apilado con la red de la página 2.

## 12. Para los colegas

Todo el mecanismo cabe en un plano. El producto punto es un coseno, W_Q·W_Kᵀ
es una rotación, la temperatura del softmax es β, y la salida de la atención es
un centro de masa dentro del círculo. Cada pieza se ve y se puede mover a mano.

## 13. Pendiente de decidir

- Decidido: el número de la página y el nombre del archivo son el 10 y
  `10-Atencion.html`.
- Mapa, galería o ambos (sección 7).
- Cómo se mueve φ, y las unidades de su deslizador (sección 8).
- El recorrido de β y su sorteo inicial (sección 8).
- La semilla y el muestreo de las 40 de prueba (sección 4).
- Si habrá una página que entrene esta red. Propuesta: no; con cuatro
  parámetros a mano la página se sostiene sola.
