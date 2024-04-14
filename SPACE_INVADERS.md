# Actividad
Realización de los ajustes necesarios para ejecutar la demo de Space Invaders
en la consola.

# Crasheo
Nos topamos con un problema en donde ocurre un crasheo en la ejecución, para
reproducirlo es necesario tener las siguientes condiciones:
- Biblioteca de esp32-ps3 junto con FabGL.
- Recibir una colsión entre el fuego enemigo y el jugador.

Esto provoca un error similar al siguiente:
```
Guru Meditation Error: Core  0 panic'ed (Unhandled debug exception). 
Debug exception reason: Stack canary watchpoint triggered (btController) 
Core  0 register dump:
PC      : 0x4009746d  PS      : 0x00060e36  A0      : 0x400957a7  A1      : 0x3fffc9f0  
A2      : 0x3ffc4728  A3      : 0x3ffc4750  A4      : 0x3ffc4750  A5      : 0x00000001  
A6      : 0x00000000  A7      : 0x00000017  A8      : 0x3ffc4730  A9      : 0x3fffc9d0  
A10     : 0x3ffbc41c  A11     : 0x3ffbc41c  A12     : 0x00000000  A13     : 0x00000000  
A14     : 0x3ffbc414  A15     : 0x80000001  SAR     : 0x00000018  EXCCAUSE: 0x00000001  
EXCVADDR: 0x00000000  LBEG    : 0x40092cf4  LEND    : 0x40092d0a  LCOUNT  : 0x00000000  


Backtrace: 0x4009746a:0x3fffc9f0 0x400957a4:0x3fffca20 0x40095754:0xa5a5a5a5 |<-CORRUPTED

  #0  0x4009746a:0x3fffc9f0 in taskSelectHighestPriorityTaskSMP at /Users/ficeto/Desktop/ESP32/ESP32S2/esp-idf-public/components/freertos/tasks.c:3436
      (inlined by) vTaskSwitchContext at /Users/ficeto/Desktop/ESP32/ESP32S2/esp-idf-public/components/freertos/tasks.c:3519
  #1  0x400957a4:0x3fffca20 in _frxt_dispatch at /Users/ficeto/Desktop/ESP32/ESP32S2/esp-idf-public/components/freertos/port/xtensa/portasm.S:436
  #2  0x40095754:0xa5a5a5a5 in _frxt_int_exit at /Users/ficeto/Desktop/ESP32/ESP32S2/esp-idf-public/components/freertos/port/xtensa/portasm.S:231


ELF file SHA256: 94b476b7a53a629f
```
Este error consiste en una pila dedicada a una tarea que se llena en todos
los casos, esto sucede en todos los casos el backtrace es el que puede llegar
a variar.

## Actividades para identificar el problema
1. ¿Cuál es la última línea de código que se ejecuta antes de crashear? Para
   esto considero necesario realizar un caso mínimo reproducible.
2. Identificar la tarea que ocasiona el crasheo. Esto debería de ser posible
   aplicando `vApplicationStackOverflowHook`, en mi caso no parece llamarse en
   ningún momento.

# Referencias
- [Documentación sobre errores fatales](https://docs.espressif.com/projects/esp-idf/en/stable/esp32/api-guides/fatal-errors.html). No la he revisado.
- [FreeRTOS: Stack and Overflow checking](https://www.freertos.org/Stacks-and-stack-overflow-checking.html)
- [why do i get the debug exception reason stack canary watchpoint triggered](https://stackoverflow.com/questions/56779459/why-do-i-get-the-debug-exception-reason-stack-canary-watchpoint-triggered-main)
