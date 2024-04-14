# Información útil
configASSERT funciona igual que el assert común de C.

# Aspectos clave de tareas
- Las tareas no deben de retornar nunca por lo general y se implementan con un
ciclo infinito.
- No es buena idea eliminar una tarea que se está ejecutando en otro núcleo,
se pueden borrar mutexes y bloques de memoria podrían no haber sido liberados.
- Tenemos funciones que empiezan con `vTask` para suspender y eliminar tareas.


# Manejo de un Stack Overflow
Para manejar stack overflows con FreeRTOS necesitamos marcar la opción
`configCHECK_FOR_STACK_OVERFLOW` dentro de los ajustes dentro de 
`FreeRTOSConfig.h`, dado que usamos IDF FreeRTOS esto debe ser configurado
desde `menuconfig` que nos provee el ESP-IDF. Después tenemos que definir la
función `vApplicationStackOverflowHook`, que tiene el siguiente prototipo.
```C
void vApplicationStackOverflowHook( xTaskHandle pxTask, signed char *pcTaskName );
```
Esta función se ejecuta cuando se detecta un stack overflow e imprimir el
nombre de la tarea que causó el overflow. En caso de estar corrupto tanto el
handler y el nombre podemos usar `pxCurrentTCB`.  
Tenemos tres métodos para manejar los stack overflows, se configuran a
través de `configCHECK_FOR_STACK_OVERFLOW` poniendo un valor entre el 0 y el
2 para escoger el método.
- El primer método revisa que hayan ocurrido un overflow al hacer un switch
context, este método llama la función anteriormente mencionada. Esto se activa
con configCHECK_FOR_STACK_OVERFLOW igual a 1.
- El segundo método parece ser el que se usa con FabGL

# Por encontrar
- ¿Qué es el switch context? Está relacionado con los hilos y las tareas,
  parece ser lo que ocurre al cambiar de tareas.

# Referencias
- https://www.freertos.org/Stacks-and-stack-overflow-checking.html
- https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-reference/system/freertos.html
- https://stackoverflow.com/questions/56779459/why-do-i-get-the-debug-exception-reason-stack-canary-watchpoint-triggered-main
