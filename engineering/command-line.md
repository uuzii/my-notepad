# Introducción a la terminal y línea de comandos

## 1. Introducción

### ¿Por qué aprender terminal y línea de comandos?
Entre muchas razones, las principales que identifico son:
* La computadora no entiende el lenguaje natural, los comandos son una de las formas más mnemónicas de comuncarnos con ella y decirle qué hacer y cómo hacerlo.
* Casi todas las cosas que se pueden hacer en la interfaza gráfica, se pueden hacer desde la terminal
* Descubriremos que hay grande poder en la terminal para automatizar tareas, administrar nuestro sistema, comunicarnos con otros sistemas o administrar sistemas remotos.

### Estrutura de un comando
Un comando no es sino un programa que ejecutaremos con la siguiente extructura general:

  `$ [nombre del programa] [parámetros] [modificadores]`

Ejemplo:
  `ls -l`
Este comando, nos enlista los archivos que se encuentran dentro del directorio actual, el parámetro `-l` nos muestra mayor información de los archivos.

### Algunos comandos básicos
* `date`: muestra la fecha actual del sistema
* `echo "Uzi"`: Imprime un texto en la terminal
* `man [comando]`: Nos muestra la *documentación* de un programa para ver todas sus opciones

### Algunas utilidades de la terminal
* Si empiezas a escribir algún comando y no recuerdas el nombre completo del programa, puedes pulsar tab para ver qué comandos empiezan con esas iniciales
* Para navegar en los comandos anteriores que ejecutaste, pulsa las teclas arriba y abajo
* El comando `history` lista un historial de los comandos que hemos ejecutado recientemente, para hacer uso de alguno de ellos, introduciremos `![num de comando]`
* Si de antemano sabemos que en el directorio actual hay un elemento nombrado con ciertas iniciales, podemos escribirlas y posteriormente pulsar tab para completar el nombre

## 2. Sistema de archivos
* La computadora se encarga de organizar nuestros archivos en la memoria, pero será nuestra tarea organizarlos a nivel superficial en carpetas

### Algunos comandos del sistema de archivos:
* `ls`: Enlista todos los directorios y archivos que existen en el directorio actual
  `-l`: Muestra más información acerca de cada archivo
  `-a`: Muestra los archivos ocultos
* `cd [directorio]`: Nos permite acceder a una directorio
  > * En todos los directorios encontraremos el directorio `..`, que es un apuntador a la directorio anterior, si hacemos `cd ..` iremos a la carpeta anterior.
  > * En todos los directorios encontraremos el directorio `.`, que es un apuntador al directorio actual, nos ayudará cuando queramos hacer referencia a la carpeta actual par ejecutar archivos o copiarlos, por mencionar algunos ejemplos.
  > El directorio `~` suele ser el directorio raíz de nuestro sistema
* `pwd` Nos muestra la ruta actual en la que nos encontramos
* `rm [archivo]`: Borra un archivo (no servirá para directorios)
  `-i`: Pregunta si estamos seguros de borrar el archivo
  `-r [directorio]': Borra recursivamente un **directorio** y los archivos que contiene
  `-f`: Forza a borrar los archivos sin preguntar
* `cp [fuente] [destino]`: Copia un archivo de una carpeta a otra
* `mv [archivo] [destino]`: Mueve un archivo de un directorio a otro
* `mkdir [nombre]`: Crea un directorio nuevo con el nombre indicado
* `touch [nombre.extension]`: Crea un archivo nuevo

### Archivos de texto plano
Son archivos *no binarios* que se pueden editar fácilmente desde la terminal pues procesan texto en tiempo real, por ello se dice que son **interactivos**. Ejemplos de ellos, son los archivos .txt, .json, código fuente en general.

### Editores de texto
Para modificar estos archivos, tenemos dos editores principales:
* **VIM**
  1. Para abrirlo, usaremos `vim [archivo]`
  2. Nos mostrará el contenido del archivo pero para empezar a editar, hay que pulsar la letra `i` para usar el *insertion mode*
  3. Para salir del *insertion mode*, pulsaremos la tecla esc
  4. Para salir del editor, introduciremos `:wq` para guardar y salir
* **Nano**
  1. Para abrirlo, usaremos `nano [archivo]``
  2. Podremos editar inmediatamente
  3. Para cualquier acción, existen comandos que comienzan por ctrl + [opcion], ejemplo: ctrl + x para salir.

### Utilidades para archivos de texto
Existen algunas utilidades en el sistema de archivos, que nos ayudarán para hacer un tratamiento de los archivos de texto, por mencionar algunas:
* `cat [file]`: muestra el contenido de un archivo
* `more [file]`: se muestra una parte del archivo permitiéndonos navegar en él, para salir, presionamos `q`
* `head [file]`: muestra las primeras líneas de un archivo
  `-n`: muestra un numero n de las primeras líneas
* `tail [file]`: muestra las últimas líneas de un archivo
  `-n`: muestra un numero n de las últimas líneas
* `grep [RegExp] [archivo]`: muestra las líneas de un archivo que contengan la expresión regular señalada
  `-i`: la búsqueda es insensible a mayúsculas
  `-c`: solo cuenta la cantidad de líneas que contienen la expresión buscada
  `-n`: busca las líneas y las numera
  `-v`: muestra las líneas complemente de nuestra búsqueda, es decir, las que no contienen la expresión introducida
* `sed`: permite hacer un tratamiento de flujo de caracteres, por ejemplo, [referencia](https://www.geeksforgeeks.org/sed-command-in-linux-unix-with-examples/)
