🚀 v0.1.0

# 1. Introducción

## ¿Por qué aprender terminal y línea de comandos?
Entre muchas razones, las principales que identifico son:
* La computadora no entiende el lenguaje natural, los comandos son una de las formas más mnemónicas de comuncarnos con ella y decirle qué hacer y cómo hacerlo.
* Casi todas las cosas que se pueden hacer en la interfaza gráfica, se pueden hacer desde la terminal
* Descubriremos que hay grande poder en la terminal para automatizar tareas, administrar nuestro sistema, comunicarnos con otros sistemas o administrar sistemas remotos.

## Estrutura de un comando
Un comando no es sino un programa que ejecutaremos con la siguiente extructura general:

```bash
$ [nombre del programa] [parámetros] [modificadores]
```

Ejemplo:
```bash
$ ls -l
```
Este comando, nos enlista los archivos que se encuentran dentro del directorio actual, el parámetro `-l` nos muestra mayor información de los archivos.

## Algunos comandos básicos
* `date`: muestra la fecha actual del sistema
* `echo "Uzi"`: Imprime un texto en la terminal
* `man [comando]`: Nos muestra la *documentación* de un programa para ver todas sus opciones

## Algunas utilidades de la terminal
* Si empiezas a escribir algún comando y no recuerdas el nombre completo del programa, puedes pulsar tab para ver qué comandos empiezan con esas iniciales
* Para navegar en los comandos anteriores que ejecutaste, pulsa las teclas arriba y abajo
* El comando `history` lista un historial de los comandos que hemos ejecutado recientemente, para hacer uso de alguno de ellos, introduciremos `![num de comando]`
* Si de antemano sabemos que en el directorio actual hay un elemento nombrado con ciertas iniciales, podemos escribirlas y posteriormente pulsar tab para completar el nombre

# 2. Sistema de archivos
* La computadora se encarga de organizar nuestros archivos en la memoria, pero será nuestra tarea organizarlos a nivel superficial en carpetas

## Algunos comandos del sistema de archivos:
* `ls`: Enlista todos los directorios y archivos que existen en el directorio actual
  * `-l`: Muestra más información acerca de cada archivo
  * `-a`: Muestra los archivos ocultos
* `cd [directorio]`: Nos permite acceder a una directorio
  * En todos los directorios encontraremos el directorio `..`, que es un apuntador a la directorio anterior, si hacemos `cd ..` iremos a la carpeta anterior.
  * En todos los directorios encontraremos el directorio `.`, que es un apuntador al directorio actual, nos ayudará cuando queramos hacer referencia a la carpeta actual par ejecutar archivos o copiarlos, por mencionar algunos ejemplos.
  * El directorio `~` suele ser el directorio raíz de nuestro sistema
* `pwd` Nos muestra la ruta actual en la que nos encontramos
* `rm [archivo]`: Borra un archivo (no servirá para directorios)
  * `-i`: Pregunta si estamos seguros de borrar el archivo
  * `-r [directorio]': Borra recursivamente un **directorio** y los archivos que contiene
  * `-f`: Forza a borrar los archivos sin preguntar
* `cp [fuente] [destino]`: Copia un archivo de una carpeta a otra
* `mv [archivo] [destino]`: Mueve un archivo de un directorio a otro
* `mkdir [nombre]`: Crea un directorio nuevo con el nombre indicado
* `touch [nombre.extension]`: Crea un archivo nuevo

## Archivos de texto plano
Son archivos *no binarios* que se pueden editar fácilmente desde la terminal pues procesan texto en tiempo real, por ello se dice que son **interactivos**. Ejemplos de ellos, son los archivos .txt, .json, código fuente en general.

## Editores de texto
Para modificar estos archivos, tenemos dos editores principales:
* **VIM**
  1. Para abrirlo, usaremos `vim [archivo]`
  2. Nos mostrará el contenido del archivo pero para empezar a editar, hay que pulsar la letra `i` para usar el *insertion mode*
  3. Para salir del *insertion mode*, pulsaremos la tecla esc
  4. Para salir del editor, introduciremos `:wq` para guardar y salir
* **Nano**
  1. Para abrirlo, usaremos `nano [archivo]`
  2. Podremos editar inmediatamente
  3. Para cualquier acción, existen comandos que comienzan por ctrl + [opcion], ejemplo: ctrl + x para salir.

## Utilidades para archivos de texto
Existen algunas utilidades en el sistema de archivos, que nos ayudarán para hacer un tratamiento de los archivos de texto, por mencionar algunas:
* `cat [file]`: muestra el contenido de un archivo
* `more [file]`: se muestra una parte del archivo permitiéndonos navegar en él, para salir, presionamos `q`
* `head [file]`: muestra las primeras líneas de un archivo
  `-n`: muestra un numero n de las primeras líneas
* `tail [file]`: muestra las últimas líneas de un archivo
  `-n`: muestra un numero n de las últimas líneas
* `grep [RegExp] [archivo]`: muestra las líneas de un archivo que contengan la expresión regular señalada
  * `-i`: la búsqueda es insensible a mayúsculas
  * `-c`: solo cuenta la cantidad de líneas que contienen la expresión buscada
  * `-n`: busca las líneas y las numera
  * `-v`: muestra las líneas complemente de nuestra búsqueda, es decir, las que no contienen la expresión introducida
* `sed`: permite hacer un tratamiento de flujo de caracteres, ejemplo, para mostrar en pantalla el contenido del archivo **input.txt**, sustituyendo la palabra *"hello"* por la palabra *"world"*, ejecutamos:

  ```bash
  $ sed 's/hello/world/' input.txt
  ```
  Ver la [referencia](https://www.gnu.org/software/sed/manual/sed.html) completa
* `awk`: Nos ayuda a buscar líneas en un archivo que contengan cierto patrón y ejecutar cierta acción en ellas. Generalmente se usa en archivos que están organizados por filas o estructurados.

# 3. Procesamiento de datos
En la terminal, podemos enlazar los procesos que hemos aprendido mediante ciertos tipos de procesamiento:
* **Redirección.** Consiste en poner un archivo como entrada de un proceso, por ejemplo, en el siguiente comando, le enviamos un archivo sql a un servidor con la finalidad de ahorrarnos el trabajo de introducir cada uno de los elementos que éste contiene:
```bash
$ mysql -h 127.0.0.1 -u root -p1234 < dump.sql
```
* **Salida.** Consiste en direccionar el resultado de un proceso hacia un archivo, en el siguient ejemplo, guardamos el listado de archivos y directorios que existen en el directorio actual dentro de un archivo llamado **list.txt**:
```bash
$ ls > archivos.txt
```
  * Una variante de este tipo de procesamiento, es usar doble signo `>>` para guardar el resultado de nuestro comando en un archivo ya existente.
* Concatenar procesos. Este tipo de procesamiento nos permite comunicar procesos, es decir: poner la salida de un proceso como la entrada de otro, en el siguiente ejemplo, mostramos en pantalla los archivos que contengan la extensión `.txt` conectando el `ls` con el comando grep (que busca las líneas que contienen cierta expresión dada):
```bash
$ ls -l | grep .txt
```

# 4. Administradores de paquetes
Son programas que nos ayudan a encontrar otros programas (paquetes) en internet y posteriormente instalarlos en nuestro sistema, dependiendo el sistema operativo, los gestores de paquetes varían, algunos de ellos son:
* `apt` en Ubuntu y Debian
* `zypper` en Suse
* `brew` MacOS

Cuando requerimos ejecutar estos gestores usando permisos de administrador, antecedemos con la palabra `sudo`, ejemplo, para instalar *lynx* que es un programa que nos permite ver páginas web en la terminal usamos:
```bash
$ sudo apt install lynx
```

## Otros administradores de paquetes
No solo los sistemas operativos poseen estos administradores, también algunas plataformas utilizan estas tecnologías para gestionar dependencias que se encuentran distribuídas en la web, algunos de ellos son:
* `pip` para python
* `composer` para php
* `npm` para node

# 5. Compresion y descompresión de archivos
Podemos comprimir archivos usando la terminal mediante los siguientes comandos:
* Comprimir:
```bash
$ gzip [file]
```
* Descomprimir:
```bash
$ gzip -d [file]
```

Para validar la eficiencia de nuestra compresión, podemos usar el comando `ls -lh` para visualizar el tamaño que tienen todos nuestros archivos en un directorio.
Listar archivos y su tamaño

## 6. Combinacion de archivos
Podemos ejecutar combinaciones de archivos mediante las siguientes opciones del programa `tar`:
* `tar cf [newFile.tar] [file1] ... [fileN]`: Para generar un nuevo archivo con la combinación de los n archivos indicados
* `tar tf [myTarFile.tar]`: Para ver el contenido de un archivo .tar
* `tar xf [myTarFile.tar]`: Para separar separar los archivos combinados en un archivo
* `tar cfz [newFile.tgz] [file1] ... [fileN]`: Para agrupar los archivos indicados y comprimirlos
* `tar xzf [myTarFile.tgz]`: Para desagrupar los archivos indicados y descomprimirlos

# 7. Búsquedas
Podemos Para buscar algun archivo desde la terminal pero siempre debemos actualizarlos mediante el comando:
```bash
$ sudo updatedb
```

Encontramos las siguientes opciones para hacer búsquedas:
* `locate [file]`: Encuentra la ruta de un archivo
* `wereis [package]`: Encuentra la ubicación de un paquete

Para hacer búsquedas más completas, existe el comando `find`, que podríamos emplear de la siguiente manera:
```bash
$ find . -user uzi -type f -perm 644
```

Mediante este comando, podemos buscar recursivamente en el directorio `.` del usuario *uzi* archivos que contengan los permisos 644, como podemos ver, este programa puede buscar mediante muchos parámetros, algunos de ellos:
* `-name [name]`: Por nombre de archivo
* `-pattern [pattern]`: Que el nombre tenga algún patrón
* `-empty`: Archivos vacíos

# 8. Interacciones en la web

## Peticiones HTTP
Para hacer **peticiones** crudas mediante el protocolo HTTP, podemos usar:
```bash
$ curl [endpoint] [-v]
```
> `-v` muestra los detalles de la comunicación vía http

## Descargas desde servidores HTTP:
Para realizar **descargas** desde seridores http, podemos emplear:
```bash
$ wget [url]
```

## Acceso seguro a otras computadoras
Para hacer conexiones a otras computadoras de manera encriptada usamos
```bash
$ ssh [server]
```
> Estas conexiones solo funcionan si se tienen las llaves públicas en que coincidan en ambos equipos si no, se tiene que especificar el usuario

## Enviar e-mails
Para enviar emails desde la terminal, podemos usar `postfix`, para empezar a usarlo, introducimos `service postfix start`. Posteriormente, para enviar un email usaremos:
```bash
$ echo "Probando" | mail -s "Probando envío de emails" your@email.com
```

# 9. Scripting en Bash
Podemos generar *scripts* que automaticen ciertas tareas de las que se mencionan en este documento y guardarlas en un archivo tipo `.sh`. Antes veamos también, que podemos crear variables que estén presentes en nuestra terminal mediante archivos que contegnan las mismas, estas se llamarán *variables de entorno*

## Variables de entorno
1. Para crear un nuevo archivo de variables de entorno creamos un archivo nuevo con  `vim .[filename]`.
2. El contenido del archivo llevaría una estructura como la siguiente:
  ```txt
  export NAME="uzi"
  ```
3. Para empezar a hacer uso de nuestras variables, introucimos:
  ```bash
  $ source .[myFile]
  ```
4. Ahora podemos usar nuestra variable introduciendo:
  ```bash
  $ $NAME
  ```

## Scripts
Podemos empezar a escribir scripts simplemente generando un archivo de extensión `.sh`, en el siguiente script, validamos que si no existe una carpeta con un nombre dado en una ruta, ésta se cree y posteriormente imprima un mensaje de éxito:
```bash
#!/bin/bash

NEW_DIR=dir

if [ ! -d "~/Documents/my/route/$NEW_DIR" ]; then
	mkdir ~/Documents/my/route/$NEW_DIR
fi

echo "`date`: Done!"
```