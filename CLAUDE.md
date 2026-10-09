# CLAUDE.md — BasicRNA

Tope de este archivo: 60 líneas. La forma de trabajar y la lista cerrada
de documentos están en `~/.claude/CLAUDE.md` y valen aquí. Este archivo
apunta, no explica: lo que dicen los comentarios del código no se repite.

## Qué es

Páginas sueltas para las primeras clases de redes neuronales. El alumno
mueve los parámetros a mano y ve qué le pasa a la frontera, o ve cómo los
mueve el algoritmo, un punto a la vez. El orden de las páginas importa.

## Pila y cómo se ejecuta

- HTML y JS nativo (binary64), canvas 2D; sin bibliotecas, CDN ni build.
- Doble clic en `index.html`. Sin suite: cada página se verifica sola al montar.

## Archivos

```
1-Perceptron.html     una neurona, tres parámetros a mano
2-EditParam.html      red 2→4→1, diecisiete a mano, ReLU o tanh global
3-EditActivFun.html   2→4→1 con activación elegida por neurona (seis)
4-Backprop.html       SGD de a un punto sobre la red de la 2; expone η
5-Convolucion.html    CNN 6×6 ya entrenada: se pinta la entrada, el filtro gira
6-BackpropCNN.html    SGD de a una imagen sobre la CNN de la 5; expone η
7-NeuronaEnElTiempo.html  una neurona en el tiempo: recuerda u olvida
8-Recurrente.html     red recurrente escalar, cinco a mano; lineal o tanh
9-BackpropTiempo.html BPTT de a una tira sobre la red de la 8; expone η
10-VariasNeuronas.html  2 o 3 neuronas de estado: paridad y residuo entre tres
11-LazoComoMatriz.html  el lazo como matriz: la señal de regreso con dos neuronas
index.html e img/     el portal; miniaturas 900×600, portada, originales/ intactos
ESPEC-n-*.md          spec previa; se borra al construir, la sustituye su cabecera
Explica.md y TODO.md  arquitecturas, fórmulas y cifras medidas; pendientes
```

## Fronteras

- No se configura: momento, lote, épocas e inicialización son de TalleRNA.
- Una página que entrena expone sólo lo que su animación vuelve visible.
- Un HTML autocontenido por página; ninguna importa código de otra.
- Descriptores verificados antes de crear lienzos; si fallan, no monta.
- Ancla con copias: no permutación; copias exactas por índice y su geometría.
- Ante la duda no se selecciona (6 px; la de OTRO índice al doble; 2.5 px aparte).
- θ se ordena i·N_oculta + j; el orden traspuesto rompe el ancla.
- Grosor contra el tope del deslizador, con raíz; nunca contra el máximo.
- Azul y rojo son el signo del peso y no se reusan para nada más.
- Todo sorteo sale de splitmix32 con semilla fija; nunca Math.random.
- La 4, la 6, la 9 y la 11 comparan su gradiente con diferencias centradas al montar.
- Se proyecta: 1 a 11 caben en 1196×940 sin desplazamiento; con barra, no (arriba).

## Conductas verificadas

- Bajo ~1185 px el tablero envuelve y salta a 1228–1533 px; la 9 ya bajo 1196.

## Dónde estamos (tres líneas; se reescribe entera)

- Las once están; la serie recurrente va de la 7 a la 11 y cierra con el lazo como matriz (D17, D19).
- Sigue: la atención, especificada en `ESPEC-11-Atencion.md` y todavía sin número (D18).
- Abierto: homogeneizar por fuera del dibujo y rehacer miniaturas; `TODO.md`, 7.
