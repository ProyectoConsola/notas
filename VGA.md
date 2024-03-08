# VGA
Video Gate Array es un estándar para transmitir información de video.
Los colores son indicados por tres puertos cuyos voltajes van de 0 a 0.7
Volts, la sincronización horizontal (H-SYNC) y sincronización vertical
(V-SYNC) siguen lógica TTL.
La siguiente imagen muestra los puertos y sus usos en un puerto VGA.
![[Pasted image 20240222164237.png]]
Cada resolución tiene un tiempo determinado de duración para los pixeles.
El dibujado de pixeles en pantalla incluye a otras secciones adicionales
cuya duración también es especificada en pixeles: pulsos horizontales,
porches frontales y porches traseros se toman en cuenta al dibujar una
línea y sus valores varían de monitor en monitor, algunas resoluciones no
tienen una especificación exacta para los pulsos y porches por lo que es
necesario "adivinar" sus valores, en caso de no tener los valores correctos
terminamos con una imagen que no se puede ver totalmente.
Saber la cantidad real de pixeles para dibujar la pantalla es necesario
para conocer también la frecuencia a la que se dibujan los pixeles visibles.

## Porches
![[Pasted image 20240222164431.png]]
La resolución pensada es 320x200, necesita de un reloj de 12.93 MHz para el dibujado de píxeles.

## Referencias
https://web.mit.edu/6.111/www/s2004/NEWKIT/vga.shtml
https://chipnetics.com/tutorials/understanding-75-ohm-video-signals/
https://lateblt.tripod.com/bit74.txt
