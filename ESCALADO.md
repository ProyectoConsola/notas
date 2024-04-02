# Motivación
En algunas interfaces como el menú principal deseamos presentar bitmaps con
pocos pixeles pero mostrados como versiones escaladas de ellos mismos.  
Características clave:
- Agrandamiento de imágenes en factores enteros, este factor multiplica el
  ancho y alto de la imágen manteniendo su ratio.
- Imágenes estáticas sin animaciones.

# Implementación: introducción
Estas fueron las consideraciones que se hicieron:
- No podemos simplemente decirle al VGAController que repita pixeles en
  pantalla ya que este se basa en lectura de bits.
- Las estructuras Sprite y Bitmap no realizan internamente nada de dibujado
  ni pueden pedir formas de dibujar distintas al controlador.
- El controlador determina la forma de dibujar, si queremos usar
  condicionales para determinar el aspecto de pixeles sin incurrir en mucho
  uso de memoria se necesita el controlador VGADirect.

Existen dos formas para hacerlo:
1. Usando el controlador VGADirect repitiendo los pixeles por la cantidad
  que se desea escalar.
2. Usando el controlador VGAController creando arreglos más grandes en
  memoria que guardarán las versiones escaladas.

# Implementación: noción general
