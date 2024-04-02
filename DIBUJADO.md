# Introducción
En este documento hablo sobre los aspectos de funcionamiento interno de la
biblioteca FabGL, listo conocimientos que fue necesario investigar para la
realización de implementaciones correctas en los videojuegos.

# Dibujado con VGAController
VGAController es una clase descendiente de BitmappedDisplayController por lo
que su mecánica de dibujado está basada en el dibujado de bitmaps.
Tenemos dos formas de dibujar bitmaps, una es a través de canvas en donde
dibujas solo bitmaps y la otra es dibujando sprites directamente al
VGAController especificando un conjunto de sprites.

## Bitmaps
Un bitmaps es un 'mapa de bits', es decir, un arreglo con bits que indican
el aspecto de una imagen, en esta biblioteca son especificados con la
estructura Bitmap, esta guarda la información del tamaño y formato en el que
está guardado sin hacer mucho más.

## Sprites
Para el caso de la biblioteca son estructuras que solo contienen datos y
métodos básicos para la alteración de sus datos internos. Estos sprites
contienen uno o más bitmaps, esto con el motivo de posibilitar animaciones.

## Primitivas
El controlador dispone de una clase interna llamada Primitive, esta es la
representación más básica de una acción que el controlador tendrá que
realizar, estas acciones están definidas en la biblioteca y no hay forma
directa de modificarlas. Los comandos para primitivas no están restringidos
a solo este controlador y son comunes a varios.  
Algunas cosas que se pueden hacer con las primitivas son: cambiar color del
lapiz, dibujado y rellenado de rectangulos, y dibujado de bitmaps.

## Dibujado de bitmaps con el controlador de video
un arreglo de sprites, donde cada sprite puede contener
uno o más bitmaps en el. Se procesan todas las primitivas antes de ejecutar
cualquier creación de sprites, se crea un buffer especial para cargar el
sprite a dibujar, este dibujado no ocurre en setSprites sino (aparentemente)
en showSprites.

## Dibujado de bitmaps con Canvas
Se envía una estructura Bitmap, a partir de aquí se crea una primitiva
describiendo a la imagen a dibujar, no se realizan operaciones complejas aquí
para obtener la primitiva ya que describir bitmaps de por si es simple. Esta
primitiva es añadida, luego es ejecutada y sus resultados son procesados para
su dibujado con showSprites.  
Podemos decir que este es un método de alto nivel para realizar dibujado
de imágenes arbitrarias comparado con tomar el controlador de pantalla
directamente. 

## Funcionalidad de más bajo nivel
Más allá de la función showSprites es que ocurre la copia en memoria de las
imágenes hacia el buffer del VGA, esta funcionalidad no lo alcancé a leer con
detenimiento así que no puedo hablar más sobre ella.

## Conclusiones
En base a lo visto podemos decir lo siguiente:
- El dibujado en pantalla con este controldaro consiste totalmente en la
  lectura y copia de bits en memoria.
- Sprites y bitmaps solo guardan información que el controlador lee de una
  forma predeterminada.
- La clase VGAController se encarga del procesamiento y alteraciones
  necesarias para el mostrado de imagenes.
- La clase VGAController desciende de BitmappedDisplayController por lo que
  toda su lógica está basada en lectura de bitmaps.
- No es posible realizar funciones que describan como crear los píxeles en
  pantalla si no es con el uso de memoria.
