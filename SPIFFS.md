# SPIFFS (SPI Flash File System)

## Introducción 
SPI es un protocolo de comunicación que nos permiten comunicarnos
en el microcontrolador y las diversas secciones de memoria.
Resaltando las secciónes de memoria, ya que es donde participa el 
fs (file System) del esp32.

Spiffs es un sistema de archivos que tiene como objetivo a dispositivos
con memoria flash SPI NOR (No volatil) en objetos embebidos

Teniendo en mente sastifacer las siguientes necesidades:
  - Bajo consumo de la Ram para objetos pequeños.
  - Borrado de secciones de memoria por bloque.
  - Fácil restablecimiento del bloque a 1s.
  - La escritura es de 1s a 0s.
  - Los ceros sólo pueden ser convertidos a 1s al borrar.
  - Nivelación del uso y desgaste.

Que si puede hacer spiffs
  - Bajo consumo de RAM.
  - Buffers de ram de tipo estatico.
  - Funciones de tipo Archivo como:
      - Abrir, Cerrar, Leer, Escribir, Buscar, estadistica.
  - Multiples configuraciones de spiffs pueden ser ejecutadas al 
    mismo tiempo
  - Comprobaciones de coherencia del fs integradas
  - Totalmente programable

Que no puede hacer spiffs
  - No admite directorios. Siendo una estructura plana:
    - e.g. no hay rutas, por lo cual un archivo será nombrado como
      *tmp/miArchivo.txt*
  - No es una pila de tiempo. Una operación de escritura puede durar
    mucho más que otra.
  - No está pensado para un fs de tamaño considerable de ~128 Mbytes
  - No detecta bloques de memoria defectuosos

## Consideraciones del fabricante [espressif Docs spiffs](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/spiffs.html)
De las mencionadas anteriormente nos recomienda no utilizar más allá 
del 75% bloque de memora asignada para spiffs.
Cuando el sistema tiende a fallar en una operacción por falta de 
energía, puede corromper el archivo y como se menciono, no verifica
la integridad o salud de los archivos.
El borrado de alguna sección de un bloque de la partición de spiffs
no indica que en su totalidad a sido borrada esa sección, por lo cual
puede dejar secciones inutilizables.

## Conclusiones
Por lo que he leido, en las diversas documentaciones, es recomendable 
unicamente leer los archivos que sean subidos al bloque spiffs, por 
la parte de que el borrado por métodos de spiffs puede ser inoportuna
y generar secciones inutilizadas, desconozco si solamente durante la 
ejecución del programa. Ya que hay herramientas que nos proporciona
la ToolChain del IDF para hacer un borrado completo del spiff.
Es posible subir cualquier tipo de archivo para su lectura, como
configuraciones definidas para el programa, podría considerarse
como un .env para definiciones que no se quieran mostrar en el
código, pero incluso puede usarse para cargar plantillas de HTML.
En dado caso tambien es recomendado no pasar del 75% de la partición
aunque otros recomiendan del 80%, prefiero trabajar con lo que comenta
el fabricante.
Los archivos no deberan demasiado grandes para la partición, ni 
demasiado pequeños, para archivos pequeños es mejor utilizar la
partición nvs, que cumple la misma función, pero con un menor espacio
o bloque de memoria.

## Ver más acerda de en:
  - [API de Almacenamiento de esp32 - Documentación Oficial](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/storage/index.html)
  - [Hoja de datos de esp32 - Seccion 3.1](https://www.espressif.com/sites/default/files/documentation/esp32_datasheet_en.pdf)
  - [Manual de referencia esp32 - Sección 7](https://www.espressif.com/sites/default/files/documentation/esp32_technical_reference_manual_en.pdf#spi)
