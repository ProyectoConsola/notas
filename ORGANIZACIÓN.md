# Organización del Hardware
En este documento se detallará la organización física de la consola, que
componentes se esperan usar y las tecnologías que usaremos usaremos.  
Las conexiones físicas se realizaran a través de los GPIOs disponibles en el
ESP32 DEVKIT V1, este kid de desarrollo dispone de 30 pines en su totalidad,
11 de los cuales se pueden usar como GPIOs sin ningún problema,
adicionalmente hay pines marcados como GPIO que solo funcionan para
lectura de datos.
- **Pines útiles solo para lectura de datos**: GPIO34, GPIO35, GPIO36 y
  GPIO39.
- **GPIOs no disponibles en este devkit**: GPIO20 y GPIO24.

## Controles
Los controles serán físicos: dos botones y un joystick para cada jugador,
siendo dos jugadores en total.  
La conexión interna será por Bluetooth a través de un mando de Playstation 3.

### Alternativa con multiplexor
A continuación se menciona una alternativa con un multiplexor que quedará
solo como una segunda opción en caso de haber alguna complicación al
implementar una entrada por el mando bluetooth.  
Entrada manejada por un multiplexor de 8 a 1 o uno de 16 a 1 en caso de
desear jugar con dos jugadores, así podemos seleccionar una de las varias
entradas para que pase por un solo canal, reduciendo así el uso total de
pines en el microcontrolador.  
El pin utilizado como canal de entrada podría ser preferiblemente uno de solo
entrada; dependiendo de si usamos controles analógicos o no, también se
necesitará que el pin tenga un ADC.

## Video
Para generar video con la tecnología escogida son necesarios 8 pines para, la
configuración predeterminada usa los pines del lado derecho del devkit.
Se tendrán tres DACs de 2-bits para dar una salida de 64 colores lo que suma
6 pines, los otros 2 pines serán usado para sincronización vertical y
horizontal.  
![Conexión ESP32-VGA](images/esp32_vga_connection_diagram.png)

## Audio
Con la información dada por la biblioteca, la salida es obligatoriamente dada
por el GPIO25. El diseño final para la salida de audio no será necesariamente
este ya que se desea un sonido de buen volumen.
![Salida de audio con el ESP32](images/schema_audio.png)


## Referencia
- [VGA output schema](http://www.fabglib.org/conf_v_g_a.html). Esquema y
  posibles configuraciones para salida de video VGA.
- [Configuring Audio port](http://www.fabglib.org/conf_audio.html). Base para
  construir el circuito generador de audio.
- [ESP32 Pinout
  Reference](https://randomnerdtutorials.com/esp32-pinout-reference-gpios/)

# Organización del software
## Separación de los juegos
Cada juego contará con sus propias escenas y estructuras para manejar las
mecánicas de los juegos, por lo que haremos una separación lógica entre los
juegos y además habrá lógica unificada común a todos los juegos que servirá
para registrar el estado de la consola. Terminaríamos con una jerarquía 
Estado -> Juego -> Escenas -> Elementos:
- Juego: es el espacio donde están declarados los controladores para pantalla
  y objetos de mayor uso entre las escenas del juego.
- Escena: Contiene la lógica pertenecientes a los objetos que estarán en
  pantalla en un solo espacio, es decir, en una sola escena.
- Elementos: elementos individuales participantes de una escena.

### Estado global
Representación lógica de los elementos consola, se pueden acceder a
objetos, funciones y clases que deban de ser comunes a todos los juegos.  
Será dada simplemente por un namespace y su inicialización
será dada con una función específica.

#### Objetos
- Controlador de VGA.
- Controlador de audio.

#### Funciones
- Función inicializadora de objetos de FabGL y esp32-ps3.

#### Clases
- Clase para representar juegos.

### Juegos
Los juegos agrupan escenas propias y a su vez también contienen variables
globales que controlan su estado. Podemos decir que un juego tendrá
las siguientes características:
- Estados. Estos pueden ser como: "El jugador perdió", "El tiempo se acabó",
  "Se está jugando en este momento".
- Escenas: Como podría ser una escena inicial, del juego principal, y otras
  adicionales como una pantalla de Game Over.
- Mediciones. Tales como puntaje y el tiempo que se lleva jugando.
- Modos: niveles de dificultad.

Los juegos tendrán implementaciones distintas cada uno y algunas mecánicas
como el conteo de tiempo no pueden ser manejadas con una clase abstracta por
lo que los juegos estarán organizados por archivos, cada juego si tendrá
acceso a un estado global pero por si mismos solo serán un conjunto de clases
y objetos dentro de un archivo.  
Será necesario:
- Tener una función que incluya la lógica de lo que usualmente iría en `loop`
  para que esta sea accesible.
- Usar los objetos globales proporcionados por el "Estado".
