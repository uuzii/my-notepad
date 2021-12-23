# ¿Por qué aprender a usar la terminal?

* **Flexibilidad.** Podemos hacer muchas tareas con ayuda de la terminal de manera sencilla: procesos, o incluso tareas automatizadas.
* **Velocidad.** No depende del rendimiento de una interfaz para ejecutar sus procesos.
* **Da cuentas de lo que hace.** Siempre nos permite identificar lo que está haciendo el sistema.
* **Invocar demonios.** Este punto puede ser negativo, pues si no sabemos utilizarlo podemos generar problemas graves en el sistema.

## ¿Qué es la terminal?
En esencia, la terminal, es una interfaza gráfica también, que representa una línea de comandos o **shell**, que es un programa que le dice al sistema qué hacer a un bajo nivel. Algunos tipos de shell son:
* Bourne shell
* Bash shell (muy común)
* Z shell (muy común)
* C shell
* Korn shell
* Fish shell
* Power shell (común para windows)

Es bueno familiarizarse con estos sistemas y sobre todo en un ambiente de linux, pues es el sistema que hallaremos en muchos sitios como en el despliegue de servidores, etc.

## Caminar en la terminal
Para aprender a caminar dentro de nuestro sistema, tenemos que entender cómo está estructurado nuestro *sistema de archivos*, no es objetivo de este curso pero es importante estudiarlo. Una de las carpetas que sí nos interesa estudiar, es la carpeta *home/* o *Users/* en mac, que es la carpeta donde encontraremos nuestros usuarios. Ahora veremos algunos comandos.

* `ls` Es un comando que nos permite listar los archivos y directorios. Algunas opciones `-l` para mostrar más detalles, `-lh` para ver detalles en un formato más legible, `-la` muestra archivos ocultos en el directorio, `-lS` para listarlos en orden de tamaño.
* `clear` Limpia todo el texto que esté presente en la terminal.
* `cd [path]` Nos permite ingresar a alguna carpeta que indiquemos. Las rutas relativas que podemos usar en todos lugares es `.` para ahcer referencia al mismo directorio y `..` para ir una ruta atrás.
* `pwd` Imprime la ruta absoluta en la que estamos parados.
* `file [fileName]` Describe un tipo de archivo que quizá desconocemos.
* `tree` Imprime todos nuestros directorios a modo de árbol de manera gráfica, para limitar el número de niveles agregamos la opción `L [n]`. 

## Manipular archivos
Para generar archivos o trabajar con ellos, usaremos los siguientes comandos:
* `mkdir [name]` Para crear un directorio, de preferencia los nombraremos sin espacios, en su defecto, usar comillas para el name. Podemos crear también múltiples archivos en un mismo comando separando los nombres por comas.
* `touch [name.extension]` Para crear archivos, también se pueden generar varios en un mismo comando.
* `cp [file] [path/name]` Para copiar archivos ya sea en una ruta o indicando el nombre a crear de un nuevo archivo.
* `mv [file/dir] [path]` Para mover un archivo de ubicación. También nos permite renombrar un archivo si indicamos otro nombre en lugar de un path.
* `rm` Para borrar un archivo. La opción `-i` hace que el borrado sea interactivo, es decir, que nos pida una confirmación al borrar el archivo. La opción `-r` hace el borrado de manera recursiva, esta se usa para directorios, pues borra todos los archivos/directorios contenidos.

## Explorar archivos
Para visualizar el contenido de un archivo de texto, podemos usar los siguientes comandos:
* `head [file]` Imprime por default las primeras líneas del archivo de texto, para modificar el número de líneas a visualizar, usamos la opción `-n [n]`.
* `tail [file]` Imprime las últimas líneas del archivo de texto, al igual se le puede indicar el número de líneas a mostrar.
* `less [file]` Nos muestra todo el contenido de un archivo de texto y nos permite hacer scroll o navegar con flechas. Algunas opciones que tiene en la línea de comandos, es poner `/[word]` para buscar una palabra. Para salir solo tenemos que ingresar la letra `q`. Esta interfaz nos permite también buscar texto escribiendo el caracter `/`.
* `open [file/dir]` Solo para mac, abre el archivo en el programa por defecto para ese tipo de archivo.
* `nautilus [dir]` Solo para linux, abre un directorio en la interfaz gráfica.

## ¿Qué es un comando?
Un comando puede ser cualquiera de estas posibilidades:
1. Un programa ejecutable. Programas compilados que regularmente encontraremos en la carpeta `/usr/bin`.
2. Un comando de utilidad de la shell. Una función propia de la línea de comandos.
3. Una función de shell. Funciones que normalmente no vienen en la línea de comandos sino que son adicionales a ella.
4. Un alias. Es un comando custom que nostros creamos para ejecutar un comando o comandos.

Podemos utilizar el comando `type` para identificar de qué tipo es un comando cualquiera, ejemplo:
```bash
type cd # cd is a shell builtin
type mkdir # mkdir is /usr/bin/mkdir
type ls # ls is aliased to "ls --color=auto"
```

### Alias
Cómo crear un alias:
```bash
alias l="ls -lh"
```

Con esto generaríamos un comando `l` que nos muestre el listado de archivos de manera completa.

### Obtener ayuda e infirmación de un comando
El comando `help` nos permite visualizar las opciones que tiene un comando.
El comando `man` nos sirve para encontrar el manual se usuario de un comando.
El comando `whatis` nos da una descripción muy resumida de un comando.

## Wildcards
Son una serie de patrones que nos permiten hacer búsquedas de manera muy avanzada, se hará mediante el comando `ls`. Ejemplos:
```bash
ls *.txt # Muestra todos los archivos de tipo txt
ls initial* # archivos que inicien con 'initial'
ls initial? # archivos con 'initial' + un solo caracter
ls initial??? # archivos con 'initial' + tres caracteres
ls [[:upper:]]* # archivos que empiecen con una sola mayúscula
ls -d [[:upper:]]* # directorios que empiecen con una sola mayúscula
ls [[:lower:]]* # archivos que empiecen con una sola minúscula
ls [ad]* # archivos que inicien con letra a o con d
```

## Redirecciones
Normalmente ingresamos un comando que genera algún output. Ahora veremos cómo controlar esos inputs y outputs a través de ciertos comandos especiales, pero primero consideremos las siguientes partes de un comando:
* **stdin (0)**. Standard in, es el input que se le da al comando, puede ser una entrada de texto o de un archivo.
* **stdout (1)**. Standard output es la salida que arroja el comando, si se produce un error, se arrojará:
* **stderr (2)**. Standard error se arroja cuando hay un error al generar el output del comando.

Ejemplos de redirecciones.
* *Imprimir* la salida de un `ls` en un archivo txt: `ls > file.txt`. No concatenará, solo sobreescribirá.
* *Concatenar* la salida de un `ls` en un archivo existente `ls >> existing.txt`.
* *Imprimir* la salida de un error al invocar un `ls` de manera fallida: `ls not-valid-file 2> error.txt`.
* *Imprimir* la salida y opcionalmente el error de un comando: `ls Documents/ > output.txt 2&>1`.
* *Introducir* información de un archivo a un comando: `script.sh < data.json`

Las redirecciones son de mucha utilidad cuando ejecutamos scripts y queremos almacenar logs de alguna actividad o error.

## Pipe operator
Es uno de los operadores más útiles, ya que nos permite, por ejemplo encadenar la salida de un comando a al entrada de otro y así generar procesos que se comuniquen entre sí.

Primeramente revisemos el uso del comando `cat`, que nos sirve para concatenar la salida de dos archivos. Ahora veamos ejemplos de uso del pipe operator:
* Concatenar la salida de un `ls` con el comando `less`: `ls -lh | less`, esto nos permitirá navegar en la salida del listado.
* Listar el contenido de una carpeta, guardar esa salida en un archivo y finalmente verlo: `ls -lh | tee output.tx | less`.
* Ponerle un filtro que ordene los elementos: `ls -lh | sort | tee output.tx | less`.