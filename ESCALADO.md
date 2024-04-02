# Motivación
En algunas interfaces como el menú principal deseamos presentar bitmaps con
pocos pixeles pero mostrados como versiones escaladas de ellos mismos.  
Características clave:
- Agrandamiento de imágenes en factores enteros, este factor multiplica el
  ancho y alto de la imágen manteniendo su ratio.
- Imágenes estáticas sin animaciones.

# Introducción
Estas fueron las consideraciones que se hicieron:
- No podemos simplemente decirle al VGAController que repita pixeles en
  pantalla ya que este se basa en lectura de bits.
- Las estructuras Sprite y Bitmap no realizan internamente nada de dibujado
  ni pueden pedir formas de dibujar distintas al controlador.
- El controlador determina la forma de dibujar, si queremos usar
  condicionales para determinar el aspecto de pixeles sin incurrir en mucho
  uso de memoria se necesita el controlador VGADirect.

Existen tres formas para hacerlo:
1. Usando el controlador VGADirect repitiendo los pixeles por la cantidad
  que se desea escalar a través de condicionales que tomen en cuenta las
  scanlines.
2. Usando el controlador VGAController creando arreglos más grandes en
  memoria que guardarán las versiones escaladas.
3. Usando `createRawPixel dentro de `VGAController` especificando uno por uno
   a los pixeles en pantalla como representar la imagen escalada.

Optamos por la tercera forma inicialmente a continuación se mostrarán los
avances que realizamos respecto a una Implementación hecha de esta forma.

# Implementación
## Formatos de Bitmaps
Los bitmaps tienen que ser leídos de distinta forma dependiendo de su formato
de guardado, dos formatos que nos importan son los siguientes:
- **Mask**. Cada bit representa a un pixel, si el bit es 1 se usará el color
  especificado, si el bit es 0 se usará transparente.
- **ABGR2222**. Un byte representa a un pixel, cada color junto con el
  transparente tienen dos bits de precisión siendo 0b11 el valor máximo y
  0b00 el mínimo.
La lectura es distinta para cada formato y en caso de querer implementar el
escalado es necesario tomarlo en cuenta.

### Lectura en formato Mask
Como la memoria RAM permite obtener los bits en bloques de ocho como mínimo,
es necesario recibirlos y guardarlos en una variable de dicho tamaño que
fungiría como caché. Parte de la lectura debe de tomar en cuenta que se está
leyendo un caché de tamaño inferior al arreglo de datos y actualizarlo
conforme se van procesando los pixeles.

### Lectura en formato ABGR2222
Esto no requiere de un tratamiento especial pues recibimos un pixel por cada
byte.

### Optimización en caché
En ambos es posible hacer una optimización donde obtenemos regiones de memoria
que tengan la misma longitud que la longitud de palabra del microcontrolador,
es decir, de cuatro bytes. En el caso del formato Mask es posible obtener
hasta 32 pixeles en una sola operación.

## Lógica de impresión
La lógica de impresión puede ir en procesar cada pixel del bitmap original o
procesar cada pixel de la región en pantalla que tendrá el bitmap escalado,
nos inclinamos por la segunda opción ya que nos dejará espacio libre para
definir funciones que mapeen el pixel físico con un pixel en el bitmap, en
base al factor de escalado.  
La lógica para dibujar el bitmap puede estar incluida dentro de una estructura
que expanda a `Bitmap` y ser un método llamado `drawScaled` recibiendo un
factor de escalado y posiciones iniciales.

### Implementación incompleta de la lógica
```c++
void drawScaledBitmap(uint16_t x, uint16_t y, Bitmap &bmp, uint8_t scale) {
    int8_t row, col;
    const int16_t scaled_height = bmp.height * scale;
    const int16_t scaled_width = bmp.width * scale;
    int8_t cached_pixel_info = 0;
    int16_t current_byte = 0;
    for (row = 0; row < scaled_height; row += 1) {
        for (col = 0; col < scaled_width; col += 1) {
            if (bmp.format == PixelFormat::Mask) {
                if (((row*col) % 8) == 0) {
                    cached_pixel_info = bmp.data[cached_pixel_info];
                    cached_pixel_info += 1;
                }
                color = cached_pixel_info & (1 << (row*col % 8));
                DisplayController.setRawPixel(x + col, y + row,
                                              cached_pixel_info  );
            }
        }
    }
}
```
La implementación no fue terminada debido al tiempo que requiere, se prefirió
una alternativa que aproveche mejor lo que ya está implementado por la
biblioteca.
