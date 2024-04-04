# Objetivo principal
Desarrollar videojuego "Pong!" para el microcontrolador ESP32 DevKit V1
haciendo uso de la biblioteca FabGL.

# Plan 
Inicialmente para simplificar el proceso, se adapta para dos jugadores
utilizando los dos joystick's del DualShock respectivamente, al concluir con
esto se propondrá el modo de un sólo jugador con una pequeña y IA que serguirá
la pelota, teniendo pequeñas fallas o inexactitudes para no hacerla tan
precisa a la hora de jugar contra ella.

## Diseño
En cuanto a el diseño del juego (sprites, textos y colores) se usará la lógica
o implementación del juego ya adaptado "Space Invaders", diseñando desde cero
todos los bitmaps.
Para la parte de la lógica general del juego se deberán emplear las funciones
del código que tienen el mismo propósito en el juego ya creado y adaptado. A
continuación listo dichos elementos lógicos del juego:
- Detección de colisiones.
- Inicialización de sprites.
- Contadores de puntajes acumulados.
- Movimentos de las "raquetas".

### Sprites
Los sprites para el juego serán en total tres:
1. Rectángulo de jugador uno.
2. Rectángulo de jugador dos.
3. Cuadrado que funcionaría como la pelota.

Todos serán de color blanco por lo que se puede usar sprites del tipo 'Mask'
que están incluidos en la biblioteca.

### Movimiento
Se deberá proponer la trayectoria de las raquetas y el movimiento en
vertical de las raquetas que serán controladas por el jugador.
- La pelota seguirá una trayectoria que dependerá de las colisiones
  que se hagan con las raquetas controladas por el jugador.

### Detección de colisiones
Existe una función en la librería que se encarga de detectar colsiónes entre
sprites, lo cual es exactamente lo que se busca conseguir.
- Una vez logrado esto ahí mismo crear la parte de acumulación de puntajes y
mostrarlos.

### Texto
Texto como el puntaje actual de ambos jugadores y el tiempo restante son
necesarios para la implementación final.

# Actividades
- [ ] Determinar como funciona la detección de colisiones entre sprites
  dentro de la clase Scene.
- [ ] Implementación de las funciones que leen la entrada a partir del mando
  de PlayStation 3.
- [ ] Crear sprites necesarios para el juego.