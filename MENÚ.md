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

## Primer diseño propuesto
El diseño pensado tiene las siguientes características:
- Un cuadro central mostrando un sprite representativo del juego.
- Un botón inferior que muestre "Escoger" o "Aleatorio" dependiendo del modo.
- Flechas del lado izquierdo y derecho de la pantalla indicando que hay más
  juegos disponibles.
- Un pequeño botón de ajustes.

Problemas encontrados:
- No se dispone de una función para "escalar" sprites, necesaria para la
  presentación del sprite. Una opción es crearla, la otra será buscar una
  alternativa.

## Diseño alternativo: para el prototipo
Pensamos en un diseño más simple consiste que podamos asegurar que funcione
sin problemas y nos permita trabajar bien. Este concepto tiene las siguientes
características que lo distinguen del anterior:
- Una lista visual con los nombres de los juegos acompañados de sprites
  pequeños representativos de cada juego.
- Una flecha indicando el juego que está siendo actualmente elegido.
- Una vez presionada una tecla o botón se ejecuta el juego seleccionado.

### Organización interna
Las posiciones de los objetos serán definidas en términos genéricos
independientes de la resolución, esperando un factor de forma similar a 16:9,
este formato está basado en porcentajes y toma en cuenta el tamaño total de
cada elemento, incluyendo a los de texto.  
La selección de juegos será con los botones de arriba y abajo, realizando una
selección cíclica si se presiona alguna dirección más allá de la cantidad de
juegos disponibles, e ignorando presiones prolongadas de botones.  
Los juegos tendrán que ser llamados una vez presionado el botón de ejecutar,
preferiblemente se liberará memoria relacionada con nuestro menú. Iniciados
los juegos ya no será posible volver a el más que reiniciando la consola.

