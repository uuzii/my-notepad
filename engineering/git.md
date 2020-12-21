# Primeros conceptos

* **Archivos de texto plano.** Son aquellos que no están en formato binario y presentan solo texto, git los maneja con precisión.
* **Directorio.** Es nuestra carpeta en el disco de nuestra computadora donde tenemos nuestros archivos.
* **Stagging.** Es una zona de la memoria ram que se crea temporalmente para trackear los archivos mediante git.
* **Repositorio local.** Es un repositorio en el que guardaremos todas las versiones de la historia de nuestro proyecto (commits).
* **Repositorio remoto.** Es un repositorio en la nube del que otros compañeros pueden leer y/o escribir para colaborar.

Estas son las áreas que manejamos en git, así como las acciones que usamos para pasar al *siguiente nivel*:
| zona   | dir   | stagging | repositorio local | repositorio remoto |
| ------ | ----- | -------- | ----------------- | ------------------ |
| accion | add > | commit > | push >            | < pull             |

* `git init`: Crear el stagging en la ram
* `git add [file]`: Agrega el archivo al stagging
* `git commit -m [message]`: Añadir una versión del stagging al repositorio local con un comentario (obligatorio)
  * `-am`: fusiona la función de add y commit en un solo comando
* `git status` Me permite ver el status de mi repo local
* `git rm [file]`
  * `—-cached`: quita un archivo del stagging pero mantiene el archivo en disco
  * `—-force`: quita los archivos del stagging y los borra del disco
* `git config`: Lista todas las opciones del git cli
* `git config —list`: Lista la configuración actual
* `git show [file]`: Muestra toda la historia de un archivo
* `git log`: Muestra una lista de todos los commits
  * `—stat`: muestra cambios específicos por cada commit
* `git diff [commit] [commit]`: Muestra las diferencias de un commit vs el contenido anterior o entre dos commits, importa mucho el orden.
* `git reset [commit]`
  * `—soft`: vuelve a un punto de la historia pero sin eliminar los cambios para que estén disponibles en el futuro
  * `—hard`: nos posiciona en un punto de la historia pero quitando todos los cambios hechos
  * `HEAD`: es un unstagging de los archivos agregados recientemente

### ¿Diferencias entre rm y reset?
El primero es referente al stagging de archivos, el segundo es referente a commits (puntos en la historia)

* `git checkout [commit]`: Ir a un commit puntual con la posibilidad de regresar al actual
* `git clone [repo]`: Trae una copia de master a una carpeta local y crea la base de datos de toda la historia de nuestro proyecto
* `git push [origin] [branch]`: Enviamos la versión final al repositorio remoto

  > Nota. El campo señalado como *origin* se refiere a un alias que le denominamos al repositorio remoto al que queremos hacer push, por default se nombra origin, pero puede llevar otro nombre.
* `git fetch`: Trae una actualización de la historia del repo remoto a nuestro repo local
* `git merge`: Fusiona en nuestro directorio el contenido del repositorio remoto
* `git pull [origin] [branch]`: Es una fusión de fetch y merge

# Ramas

**Ramas.** Son formas en las que podemos hacer cambios sin afectar la rama principal.
* `git branch [branch]`: Para crear una rama en el ultimo commit en el que estamos
* `git checkout [branch]`: Para apuntar el HEAD apuntando a nuestra nueva rama
* `git merge [branch]`: Fusiona la rama en la que estamos parados con la rama que le indiquemos

**Conflictos.** Se generan cuando se superponen cambios en las mismas líneas. Para resolverlos, tenemos que identificar cuáles son los cambios que queremos conservar y generar otro add + commit.

* `git branch`: Lista todas las ramas que está registradas en el repositorio local.	
* `git show-branch —all`: Muestra las ramas que existen y una historia reciente de las mismas
* `gitk`: Abre un software que nos permite ver la historia de nuestras ramas de manera muy visual

# Guardar cambios para recuperarlos después

* `git stash`: Nos permite guardar temporalmente lo que estamos modificando pero no estamos listos para hacer un commit. Una vez ejecutado el comando, el código actual modificado se guarda en memoria y tenemos libertad para hacer checkout a otra rama sin preocupaciones.
* `git stash list`: Nos muestra los stash que tenemos activos
* `git stash pop`: Vuelve al estado modificado en el que eventualmente ejecutamos un stash. Preferentemente se tiene que ejecutar sobre la rama en la que generamos el stash.
* `git stash [branch]`: Nos permite capturar nuestros cambios temporales en una rama después de que hemos ejecutado git stash
* `git stash drop`: Nos permite deshacernos de un stash guardado en memoria

# Cherry pick

* `git cherry-pick [commit]`: Nos permite traer hacia la rama en la que ejecutamos, un commit que esté en otra rama.

El *cherry-pick* es una práctica que no se recomienda usar en ramas de producción, pues podría traer partes muy puntuales de una historia y no la rama completa, de modo que al fusionar ramas se pueden generar conflictos, es mejor fusionar ramas completas.

# Reconstruir commits

Puede darse el caso en el que hayamos agregado cambios al repo local pero éstos aún no están completos, para agregar más cambios a al repo (un commit ya realizado) ejecutamos:

* `git commit —ammend`: Nos permitirá agregar cambios del stagging al repo local y modificar el mensaje del commit.

# Reflog

Si por alguna razón hemos guardado en nuestro repositorio local cambios que eliminan información importante o incluso ramas, podemos revisar toda la historia de nuestro proyecto y luego devolvernos a ese punto ejecutando:

`git reflog`: Muestra una lista de todos los commits con sus hash’s y sus HEADs
`git reset —HARD [HASH[n]]`: Nos permite volver a un HEAD específico en la historia de nuestro repositorio. Si volvemos a un punto con la bandera `—HARD`, se elimina toda la historia que sucedió después del HEAD señalado.

En general, esta no es una buena práctica, solo se debería utilizar en casos de emergencia, en los que hayamos perdido información importante.

# Buscar en archivos y commits

* `git grep [word]`: Nos permite buscar una palabra dentro de los archivos de un repositorio.
  * `-n`: nos muestra las líneas en las que aparece nuestra palabra
  * `-c`: Nos hace un conteo de las apariciones de la palabra buscada
* `git log -s “[word]”`: Nos muestra una lista de todas las apariciones de una palabra en nuestros commits

# Comandos y recursos colaborativos en Git

* `git short log`: Nos muestra cuántos commits ha hecho cada persona que ha colaborado en el proyecto
  * `-sn`: muestra solo el número de commits de cada persona
  * `—all`: muestra todos los commits que se han hecho, incluso los borrados
  * `—no-merges`: muestra los commits que realmente se han hecho, sin mezclar los que se han obtenido de otros colaboradores
  > Nota. Estas opciones no son excluyentes, se pueden usar a la vez
* `git config —global alias.[alias] “[git command]”`: Crea un alias a un comando de git, lo podemos usar para comandos muy largos como el anterior.
* `git blame [file]`: Nos lista todas las líneas de un archivo con la persona respectiva que hizo la última modificación
  * `-c`: Hace una indentación más exacta del archivo cuando queremos ver el blame
  * `-L[line],[line]`: Nos mostrará solo el blame de las líneas señaladas
*`git branch -r`: Nos permite ver las ramas remotas a nuestro repositorio local

# Limpiar un repositorio de archivos indeseados

* `git clean —dry-run`: Lista los archivos que no se han agregado al stagging y que podemos eliminar posteriormente.
* `git clean -f` Elimina los archivos que no hemos agregado al stagging (no aplica para directorios ni extensiones registradas en el .gitignore)


# GitHub

GitHub es una plataforma que nos permite subir nuestros repositorios a la web y colaborar con otros mientras descargamos otros repositorios y subimos cambios a estos.

* `git remote add origin [url]`: Usamos este comando para apuntar nuestro repositorio local a un repositorio remoto en GitHub, para encontrar la url, pulsamos en el botón “Clone or download”
* `git remote -v`: Nos mostrará el repositorio remoto al cuál está apuntando nuestro repositorio local tanto para el fetch como para el push
* `git push origin [remote branch]`: Hace un upload de la historia de nuestro repositorio local al repositorio remoto en una rama que señalemos

GitHub también nos permite hacer cambios con el botón de editar y nos permite añadir puntos nuevos a nuestra historia remota, pero en la historia local tendríamos que hacer un pull de dichos cambios para contar con ellos.

# Llaves públicas y privadas

Para poder enviar mensajes cifrados, existe un proceso algorítmico en el que se crean dos llaves vinculadas matemáticamente: la llave pública y la llave privada (que solo la tenemos nosotros), por otro lado, la llave pública, que es un proceso matemático, puede estar publicada en cualquier lugar, pero solo nuestra llave privada tendrá la forma de descifrar los mensajes enviados. Este proceso funciona de manera bidireccional para cifrar datos y descifrarlos mutuamente entre máquinas.

Al conectarnos a GitHub, usamos generalmente un usuario y contraseña que viajarán sobre HTTPS, pero un método mas fuerte de seguridad, es usar llaves privadas y públicas. Utilizaremos un protocolo llamado **SSH**.

El proceso consistirá en:
1. Registrar una llave pública en GitHub
2. GitHub nos enviará su propia llave pública y la registraremos
3. Al ambos puntos tener la llave del otro, podrán comunicarse de manera segura.

* `git config —global user.email “my@email”`:Nos ayudará para configurar el email con el que nos identificaremos en la web a través de git
* `ssh keygen -t rsa rsa -b 4096 -c “email”`: Nos ayudará a generar un par de llaves pública y privada con determinada complejidad asociada a nuestro correo y las guarda en la siguiente ruta ~/.ssh
* `eval “$(ssh-agent -s)”`: Nos permite verificar el estatus de ssh en nuestra máquina
* `ssh-add -K [private key path]`: Registrará la llave privada que le indiquemos en nuestra máquina
* `ssh-add -l `: Muestra las llaves públicas que tenemos registradas

Una vez que tengamos dadas de alta y registradas nuestras llaves, vamos a GitHub > configuraciones > SSH > añadir una nueva llave, copiamos nuestra llave pública y la pegamos en ese sitio. Posteriormente copiamos la url de SSH de nuestro repositorio remoto, para cambiarle el origin  a nuestro repositorio local.

`git remote set-url origin [ssh]`
Elimina el *origin* de nuestro repositorio local, que es el que apunta a nuestro repositorio remoto y lo conecta con nuestro repositorio remoto mediante ssh.

# Tags y versiones

* `git log —all —graph —decorate —oneline`: Nos muestra una historia de nuestro repositorio de manera gráfica y resumida
* `alias [name]=“[command]”`: Nos ayudará para comprimir un comando en una variable o colocarle un alias
* `git tag -a v0.1 -m “[message]” [commit]`: Para agregar una versión (tag) en un commit puntual
* `git tag`: Para listar todos los tags que tenemos en el repositorio actual
* `git show-ref —tags`: Muestra una lista de los tags que tenemos con su commit relacionado.
* `git push origin —tags`: Empuja hacia el repositorio remoto los tags que hemos agregado en nuestro repositorio local. Tomar en cuenta que localmente no aportan nada los tags, pero en el repositorio remoto nos ayudarán a versionar nuestro código.
* `git tag -d [tag]`: Sirve para borrar localmente un tag, tenemos que hacerle push para que se borre en el repositorio local pero no se borrará del repositorio remoto.
* `git push origin :refs/tags/[tag]`: Este comando empujará al repo remoto la referencia guardada localmente del tag borrado.


# Flujo de trabajo

Podemos ejecutar el comando git merge localmente y luego subir el resultado al repositorio remoto para fusionar nuestras ramas.

En la industria, se manejan por lo general las siguientes ramas:

* *master*. Es la rama del proyecto en producción, que generalmente está protegida para que nadie pueda fusionarlas
* *develop*. Es la rama en la que se encuentra el proyecto que se prueba localmente antes de subir a producción
* *feature*. Son ramas que usan los integrantes del equipo para trabajar en sus partes del proyecto
* *fix*. Son ramas que se utilizan para solucionar bugs

Master y develop, constantemente deben estar sincronizadas, lo cual es trabajo del líder del equipo.

Para que otros colaboradores puedan agregar sus trabajos hacia las ramas principales, harán uso del *pull request* que no es más que una solicitud **en el repositorio remoto** para fusionar ramas feature o fix con ramas protegidas (generalmente develop o master).

# Forks

Si no somos colaboradores de un proyecto u  organización, podemos ingresar a GitHub para observar los cambios que tengo hacerle un fork, que es generar una copia del estado actual de un proyecto, esta copia se generará como un repositorio completamente nuestro en el que podemos hacer todos los cambios que queramos (incluyendo push a la rama master).

Una vez que tengamos cambios en nuestro propio repositorio forkeado, podemos hacer un pull request hacia el repositorio original y desde una a otra rama concreta.

Esta opción nos sirve para colaborar en proyectos de código abierto.

* `git remote add [upstream] [url]`: Nos permite agregar un nuevo “origin” a nuestro repositorio local para hacer fetch y push le coloca el alias [upstream].
* `git remote -v`: Nos permite ver los repositorios remotos relacionados a un repositorio local tanto para fetch como para push.


# Enviar a producción

Si configuramos un servidor y en él hacemos un pull de nuestro repositorio, podremos desplegar en la web una página sencilla, para proyectos más complejos que requieran procesos en el despliegue, usar Jenkins.


# Ignorar archivos

No todos los archivos que tenemos en un repositorio local tienen que subirse al repositorio remoto, para eso existen los .gitignore, este archivo se guarda en la raíz de nuestro proyecto, contendrá una lista de los archivos que se han de ignorar, ejemplo:

```txt
*.jpg
/node_modules
```

Ignorará todos los archivos con extensión jpg, esto mayormente se usa en archivos binarios o en dependencias instaladas desde un gestor de paquetes.

# GitHub Pages

*pages.github.com* es un sitio que nos permite tener hosting gratis. Para hostear un sitio aquí, tenemos que seguir los siguientes pasos:

1. Crear un repositorio que lleve nuestro nombre de usuario + github.io
2. Agregar algún index.hmtl a nuestro repositorio remoto, que sería el índex de nuestro sitio
3. Ir a los ajustes de nuestro repositorio remoto y buscar la sección GitHub Pages y configurar en Source la rama que queremos desplegar
4. Ir a la url minombre.github.io/index.html
