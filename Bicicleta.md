# Introducción y objetivo
El propósito principal es generar energía a partir de ella (al menos eso es lo
especificado para la exposición). Datos clave:
- La energía será generada por un alternador y esta podrá ser medida y
  utilizada.
- La velocidad será medida independientemente a través de un encoder
  fotoeléctrico colocado en el alternador.
- Un motor de pasos controlará la dificultad de pedaleo oponiéndose al pedaleo
  hacia adelante a través del freno.

Junto al sistema de la bicicleta se usará la consola de videojuegos para
arrojar datos útiles como la velocidad y energía generada.

## Funcionamiento del encoder fotoeléctrico
Este es un dispositivo sensible a la luz, cuenta con un emisor de luz
infrarroja y un receptor (este arreglo emisor-receptor se suele llamar
_optoacoplador_). Cuando un objeto corta la luz entre el emisor y el
receptor sucede un cambio de estado, estas señales tendrán forma de pulsos
TTL.  

# Software
## Medidor de velocidad y control de dificultad
En la videoconsola se mostrará una interfaz gráfica mostrando la velocidad
actual a la que se está pedaleando y se mostrará también una selección de
dificultad de pedaleo.  
- La velocidad será en función de la frecuencia de los pulsos enviados por el
  encoder. Esta necesitará ser calibrada.
- La dificultad de pedaleo indicará al motor de pasos con que fuerza presionar
  el freno de la bicicleta.

## Videojuego complementario
Juego donde necesitas pedalear físicamente para desplazarte por un mapa
predeterminado saliendo de un lugar A y llegando a un lugar B.
Elementos clave del juego:
- Vista cenital donde jugador es visto como un punto en el mapa.
- El mapa es una versión simplificada de la UMSNH.
- Conjunto preestablecido de puntos de partida y destinos, tales que
  un niño pueda llegar a ellos en 90 segundos o menos.
- Secciones con mayor dificultad de pedaleo.
- Incluir velocidad actual.
- Leaderboard con jugadores.
- Se puede girar libremente pero nunca ir más allá de los tramos definidos.  
Este juego no es absolutamente necesario y será solo empezado si terminamos
los desarrollos del software para antes de abril.

### Requisitos
- Medición analógica de giros realizados con la bicicleta.
- Creación de un mapa de bits que pueda ser mostrado en pantalla que sea
  legible.
- Definición de zonas transitables en el mapa, lo que implica también
  detección de colisiones.

# Consideraciones
- Toda señal que reciba el microcontrolador no debe de pasar de 3.3V.

