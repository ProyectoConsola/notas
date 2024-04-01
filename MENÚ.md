# Concepto
La consola tendrá un menú que se usará en dos modos: escoger directamente,
escoger aleatoriamente (como una ruleta).
En el tianguis de la ciencia, los juegos serán escogidos aleatoriamente.
Características principales:
- Escoger un número aleatorio desde el principio para escoger el juego a
  ejecutar.
- Botón de intentar otra vez.
- Retroalimentación visual donde se ven los juegos siendo escogidos. No es
  necesario ser una ruleta, formas como nombres siendo iluminados uno a la
  vez hasta detenerse en uno específico es suficiente retroalimentación.

## Diseño
El diseño pensado entonces incluye:
- Un cuadro central mostrando un sprite representativo del juego.
- Un botón inferior que muestre "Escoger" o "Aleatorio" dependiendo del modo.
- Flechas del lado izquierdo y derecho de la pantalla indicando que hay más
  juegos disponibles.
- Un pequeño botón de ajustes.

# Función de escalado
En el menú deseamos presentar sprites que representen al juego, estos sprites
contienen pocos pixeles pero estos deberán de ser mostrados como más grandes,
es necesaria una función de escalado.
Inicialmente la idea es simplemente expandir la clase Sprite, que toma un
bitmap, este sprite al momentos de dibujarse en pantalla deberá de expandirse
simplemente repitiendo pixeles.
Analizando el código resulta que:
- Un sprite es solo una colección de bitmaps.
- Es una estructura y no una clase, 
- La lógica de dibujado para el sprite es realizada por el método
  BitmappedDisplayController::setSprites
