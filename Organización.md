# Organización del Hardware
## Controles
Los controles serán físicos: dos botones y un joystick para cada jugador,
siendo dos jugadores en total.
La conexión interna será por Bluetooth a través de un mando de Playstation 3.

## Video
Para generar video con la tecnología escogida son necesarios 8 pines para, la
configuración predeterminada usa los pines del lado derecho del devkit.
Se tendrán tres DACs de 2-bits para dar una salida de 64 colores
($(2^2)^3=64$) lo que suma 6 pines, los otros 2 pines serán usado para
sincronización vertical y horizontal.

![[Pasted image 20240220140753.png]]

## Audio
El audio aparentemente solo es posible por GPIO 25.

## Organización de pines
Las conexiones físicas serán de audio y video, esto se realizará a través de
los GPIOs disponibles en el ESP32 DEVKIT V1. Este devkit dispone de 30 pines
en su totalidad, 11 de los cuales se pueden usar como GPIOs sin ningún
problema, adicionalmente hay pines marcados como GPIO que solo funcionan para
lectura de datos.

### Pines no disponibles
Pines marcados como GPIO que no son accesibles en este devkit son los GPIO 20
y 24.

### Entrada del usuario
La entrada de usuario será realizada vía Bluetooth. A continuación se menciona
una alternativa con un multiplexor que quedará solo como una alternativa.
Será manejada por un multiplexor de 8 a 1 como el CD4051BE, o uno de 16 a 1 como el CD74HC4067, así podemos seleccionar una de las varias entradas para que pase por un solo canal, la elección del multiplexor dependerá de si queremos uno o dos jugadores.
#### Selección
Selección implicará específicamente mandar salidas, entonces usaremos de entre tres y cuatro GPIOs si queremos usar más de 8 entradas.
#### Entrada
La entrada puede ser por un pin que solo acepte entrada como el GPIO 34 pero se tendrá que considerar que algunas entradas pueden ser analógicas
- Todos los GPIO de solo entrada son también ADCs.

## Referencia
- [VGA output schema](http://www.fabglib.org/conf_v_g_a.html). Esquema y
  posibles configuraciones para salida de video VGA.
- [Configuring Audio port](http://www.fabglib.org/conf_audio.html). Base para
  construir el circuito generador de audio.
