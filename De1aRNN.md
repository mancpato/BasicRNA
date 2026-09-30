Antes de los dibujos va la pregunta que motiva todo, y conviene hacérsela al grupo tal cual: *¿cómo le das a una red una frase, una señal o una serie de temperaturas, si no sabes cuánto mide y el orden importa?* El perceptrón y la red de la página 2 reciben un número fijo de entradas. Un perceptrón de 7 entradas puede leer una tira de 7, pero no una de 9. La página 5 ya dio la pista: usar el mismo filtro en todas las posiciones. La recurrente hace lo mismo en el tiempo: usa la misma neurona en cada paso.

**Qué decir en cada dibujo.**

1. **El perceptrón.** Lo conocen: z = w₁x₁ + w₂x₂ + b, tres parámetros.
2. **La neurona con su salida de regreso.** Cambias x₂ por h₍ₜ₋₁₎ y queda z = w·xₜ + u·h₍ₜ₋₁₎ + b. Son los mismos tres parámetros con otros nombres, y el lazo es lo único nuevo. A la derecha hay otra neurona pequeña, v y c, que lee el estado al final: la salida de siempre.
3. **La red desenrollada.** El lazo no se puede calcular de golpe, porque para tener hₜ primero hace falta h₍ₜ₋₁₎. Desenrollar es escribir el lazo a lo largo del tiempo. Queda algo que parece una red profunda con tantas capas como píxeles, pero todas las copias llevan los mismos números.

Ése es el diagrama de la página 7, y así se explica por qué se ve distinto. En las páginas anteriores, izquierda a derecha era avanzar de capa en capa. Aquí es el tiempo, y cada columna es la misma red pequeña. Todo lo demás se conserva: azul y rojo para el signo, el grosor, los triángulos, el glifo dentro del nodo, la sigmoide de salida, ŷ ≥ 0.5 y el contador.

**Una frase que ordena todo lo demás:** *todo lo que la red sabe del pasado cabe en un solo número, h.* Cada tarea exige guardar una cosa distinta en ese número. El último no pide guardar nada. La mayoría pide guardar una cuenta. El primero pide guardar un bit.

**A mano, antes de abrir la página.** Con w = 2 y b = −1, cada 1 suma +1 y cada 0 suma −1. Que calculen el estado de dos tiras con tres valores de u:

| tira | u = 0 | u = 1 | u = 2 |
|---|---|---|---|
| 1011 | 1, −1, 1, **1** | 1, 0, 1, **2** | 1, 1, 3, **7** |
| 0111 | −1, 1, 1, **1** | −1, 0, 1, **2** | −1, −1, −1, **−1** |

Con u = 0 sólo queda el último píxel. Con u = 1 el estado es la cuenta. Con u = 2 el primer píxel pesa más que todos los demás juntos: en 0111, −8 + 4 + 2 + 1 = −1, y por eso la segunda tira da negativo aunque tenga más unos. A los alumnos con más matemáticas puedes decirles que es Horner: el estado es la tira leída como número en base u.

**Luego, la página, en este orden:**

1. **Las copias.** Clic en una flecha u: se iluminan las seis, y el mando dice «6 líneas, un solo número». Aquí conviene preguntar cuántos parámetros tendría la red con una tira de mil píxeles. Siguen siendo cinco.
2. **Tres memorias con un solo número.** Pon la solución de «El último» y luego cambia a «La mayoría» sin tocar nada: el contador salta a 44, porque es la misma red con otra pregunta. Mueve sólo u hasta 1 y el contador llega a cero. Pasa a «El primero», lleva u a 2 y también llega a cero. De una tarea a otra sólo cambió u. En el panel se ve la diferencia: con u chico las líneas brincan con cada píxel, con u = 1 caminan, y con u = 2 deciden desde el primer píxel.
3. **La prueba con tiras más largas.** En la mayoría, con u = 0.94, el contador de entrenamiento marca 0 y el de prueba marca 1. Recordar más tiempo exige más precisión. Es la primera vez en el directorio que la prueba enseña algo que el entrenamiento no ve.
4. **El interruptor.** En «El primero» con u = 2, cambia a tanh: el contador salta de 0 a 30. Pide que suban u hasta volver a cero; hace falta pasar de 2.5. La lineal guarda el bit creciendo: la tira 1111111 lleva su estado a 127. La tanh no puede crecer, y guarda el bit quedándose en uno de sus dos valores de reposo.
5. **La mayoría con tanh.** La solución duda, con ŷ cerca de 0.62 en los casos peores. La pregunta para el grupo es por qué, y la respuesta es que para contar la tanh tiene que quedarse en su tramo casi recto.

**Tres confusiones que vale la pena anticipar:**

- **«Hay siete neuronas».** No: es una neurona dibujada siete veces.
- **«u conecta dos neuronas distintas».** No: conecta la misma neurona en dos momentos seguidos.
- **«La red ve la tira completa».** No: ve un píxel a la vez, y del pasado sólo tiene h.

**Lo que no conviene prometer.** Las redes recurrentes de verdad guardan un vector, no un número. Muchas además dan una salida en cada paso; el panel ya lo insinúa, porque es justo eso. Y las LSTM y GRU existen por el problema que va a enseñar la página 8, que es un buen cierre para la clase: el mismo u que decide la memoria hacia adelante también decide qué tan lejos llega el error hacia atrás.
