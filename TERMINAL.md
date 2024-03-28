# Control externo
Será necesario tener formas de controlar externamente el microcontrolador,
hacer que reciba actualizaciones o activar y desactivar características.

# Comunicación serie
Además de solo imprimir información por el puerto serial del microcontrolador,
es posible enviar comandos directamente al microcontrolador a través de una
consola ejecutándose directamente en el dispositivo.  
Los componentes `cmd_nvs`, `cmd_system` y `cmd_wifi` serán útiles para
realizar este desarrollo. Estos fueron hechos para un ejemplo de línea de
comandos y no son componentes disponibles directamente por el framework por
lo que tienen que ser instalados manualmente.

## Referencias
- [Console examples](
  https://github.com/espressif/esp-idf/tree/master/examples/system/console).
