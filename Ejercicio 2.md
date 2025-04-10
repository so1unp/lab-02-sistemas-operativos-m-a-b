# Sistemas Operativos - Laboratorio 2 - Mariano Bonansea, Agustin Medina, Brandon Amarillo
## Ejercicio 2 - Llamadas al Sistema

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

Al ejecutar el comando `echo hola` en xv6 con la traza activada, se observan las siguientes llamadas al sistema:


```bash
$ trace 1
[22] sys_trace: 0
[3] sys_wait: 3
$[16] sys_write: 1
 [16] sys_write: 1
echo hola
[5] sys_read: 1
[5] sys_read: 1
[5] sys_read: 1
[5] sys_read: 1
[5] sys_read: 1
[5] sys_read: 1
[5] sys_read: 1
[5] sys_read: 1
[5] sys_read: 1
[5] sys_read: 1
[1] sys_fork: 4
[12] sys_sbrk: 16384
[7] sys_exec: 0
h[16] sys_write: 1
o[16] sys_write: 1
l[16] sys_write: 1
a[16] sys_write: 1

[16] sys_write: 1
[3] sys_wait: 4
$[16] sys_write: 1
 [16] sys_write: 1
```

1. **`sys_wait`**: Se utiliza para esperar a que los procesos hijos terminen su ejecución. Esto ocurre antes y después de ejecutar el comando `echo`.

2. **`sys_write`**: Esta llamada se invoca varias veces para escribir en la salida estándar (`stdout`). En este caso, se utiliza para mostrar el prompt (`$`), el texto `echo hola`, y cada carácter de la palabra `hola` en la salida estándar.

3. **`sys_read`**: Se invoca repetidamente para leer la entrada estándar (`stdin`). Aunque no se proporciona entrada adicional en este caso, el shell realiza estas llamadas para verificar si hay datos disponibles.

4. **`sys_fork`**: Se invoca para crear un nuevo proceso hijo que ejecutará el comando `echo hola`. Esto es necesario porque el shell utiliza un modelo de bifurcación para ejecutar comandos.

5. **`sys_sbrk`**: Esta llamada se utiliza para ajustar el tamaño del espacio de memoria del proceso. En este caso, se asigna memoria adicional al proceso hijo.

6. **`sys_exec`**: Se invoca para reemplazar el contenido del proceso hijo con el programa `echo`. Esto permite que el proceso hijo ejecute el comando `echo hola`.
