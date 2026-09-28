# CLAUDE.md — BasicRNA

Tope de este archivo: 60 líneas. La forma de trabajar y la lista cerrada
de documentos están en `~/.claude/CLAUDE.md` y valen aquí. Este archivo
apunta, no explica: lo que dicen los comentarios del código no se repite.

## Qué es

Páginas sueltas para las primeras clases de redes neuronales. El alumno
mueve los parámetros a mano y ve qué le pasa a la frontera, o ve cómo los
mueve el algoritmo, un punto a la vez. El orden de las páginas importa.

## Pila y cómo se ejecuta

- HTML y JavaScript nativo (binary64) con canvas 2D; sin bibliotecas ni CDN.
- Entorno: ninguno; no hay build. Cualquier navegador moderno.
- Ejecutar: doble clic en `index.html`. Pruebas: no hay suite; cada página
  se verifica sola al montar (el ancla y, en la 4, el gradiente).

## Archivos

```
index.html            portal con las miniaturas de img/
1-Perceptron.html     una neurona, tres parámetros a mano
2-EditParam.html      red 2→4→1, diecisiete a mano, ReLU o tanh global
3-EditActivFun.html   2→4→1 con activación elegida por neurona (seis)
4-Backprop.html       SGD de a un punto sobre la red de la 2; expone η
img/                  miniaturas 900×600 y portada; originales/ intactos
```

## Fronteras

- No se configura: momento, lote, épocas e inicialización son de TalleRNA.
- Una página que entrena expone sólo lo que su animación vuelve visible.
- Un HTML autocontenido por página; ninguna importa código de otra.
- Descriptores verificados antes de crear lienzos; si fallan, no monta.
- Ante la duda no se selecciona (6 px; segunda al doble; 2.5 px aparte).
- θ se ordena i·N_oculta + j; el orden traspuesto rompe el ancla.
- Grosor contra el tope del deslizador, con raíz; nunca contra el máximo.
- Azul y rojo son el signo del peso y no se reusan para nada más.
- Todo sorteo sale de splitmix32 con semilla fija; nunca Math.random.
- La 4 compara su gradiente con diferencias centradas antes de montar.
- Se proyecta: la 4 cabe en 1196×940 sin desplazamiento; las nuevas, igual.

## Conductas verificadas

Ninguna registrada todavía.

## Documentos

Los permitidos son los de la lista cerrada del global. No hay `spec.md`.
Además, `Explica.md`: arquitecturas, fórmulas, datos y cifras medidas.
Además, `TODO.md`: pendientes con su razonamiento, para quien revise.

## Dónde estamos (máximo 6 líneas; se reescribe entera)

- Último commit: el que agrega este archivo y el plan de la CNN (TODO §4).
- Lo siguiente: la página 5 (CNN a mano), en sesión aparte.
- Abierto: README en 85 líneas contra tope 40.
