# Bicicleta
## Complemento de software en el microcontrolador
Juego donde necesitas pedalear físicamente para desplazarte por un mapa
predeterminado saliendo de un lugar A y llegando a un lugar B.
Elementos clave del juego:
- Vista cenital donde jugador es visto como un punto en el mapa.
- El mapa es una versión simplificada de la UMSNH.
- Conjunto preestablecido de puntos de partida y destinos, tales que
  un niño pueda llegar a ellos en 90 segundos o menos.
- Aumento de dificultad para el pedaleo controlado por software.
- Mostrar velocidad actual.
- Leaderboard con jugadores.
- Se puede girar libremente pero nunca ir más allá de los tramos definidos.

Para cumplir con las mecánicas establecidas será necesario:
- Un alternador que gire y genere voltaje, este podría ser el responsable de
  aumentar la dificultad de pedaleo.
- Encoder encargado de enviar trenes de pulsos TTL al microcontrolador, entre
  más alta es la frecuencia más rápido se estará pedaleando. Posiblemente la
  mejor solución es usar un contador de pulsos PCNT para medir la información
  del encoder.
- Adaptar un mapa a bitmap para su correcta visualización.
- Definir tramos transitables por el jugador en el mapa.

Puntos a consdirerar:
- El encoder no debe pasarse de 3.3 Volts, pues es el límite del ESP32
- Los saltos de un OFF a ON tienen que ser lo suficientemente rápidos como
  para que el ESP32 los detecte.
