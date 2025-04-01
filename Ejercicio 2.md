# Ejercicio 2 - Llamadas al Sistema

## Traza de llamadas al sistema en Linux

### 1. ¿Cuantas llamadas al sistema invoca el programa en cada caso?

`strace bin/sum 1 2 3 > /dev/null` utiliza **35** calls, mientras que `echo "1 2 3" | strace bin/sum > /dev/null` utiliza **38** calls.

### 2. Identificar las funciones en el código de sum.c que invocan llamadas al sistema.

Las funciones en el código de sum.c que invocan llamadas al sistema son `printf()`, `exit()` y `getchar()`.

### 3. Describir los parámetros que utiliza la llamadas al sistema identificadas.

1. `write` (invocada por `printf`)
   * **fd**: Descriptor de archivo donde escribir (en este caso 1 que es `STDOUT_FILENO`).
   * **buf**: Puntero al buffer con los datos a escribir
   * **count**: Cantidad de bytes a escribir

2. `exit_group` (invocada por `exit`)
   * **status**: Código de salida del proceso (0 para `EXIT_SUCCESS`) 

3. `read` (invocada por `getchar`)
   * **fd**: Descriptor de archivo desde donde leer (en este caso 0 que es `STDIN_FILENO`)
   * **buf**: Puntero al buffer donde almacenar los datos leídos
   * **count**: Cantidad máxima de bytes a leer
  
## Traza de llamadas al sistema en xv6
### 1. Ejecutar el comando `echo hola`. Explicar por que se invocan las llamadas al sistema que se presentan en la traza.