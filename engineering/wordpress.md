# Wordpress

Wordpress es un gestor de contenido escrito en PHP, es decir que nos permite gestionar texto o imágenes sin tener que tocar código; éste funciona con MySQL.

Wordpress proviene de un fork hecho a *b2/cafelog*, que era una herramienta de software libre, gracias a ello evolucionó y ha evolucionado a lo largo de los años hasta ahora, donde podemos hacer blogs, tiendas o incluso aplicaciones web.

# ¿Dónde hallamos Wordpress?
* `wordpress.com`: Es un servicio online que nos permite crear una cuenta para generar un sitio desde el navegador.
* `wordpress.org`: Es un sitio que nos permite descargar el código fuente de wordpress para instalarlo en un servidor con las personalizaciones que queramos añadir.

# Funcionamiento de wordpress
Cuando alguien entra a nuestro sitio, ejecuta el código fuente de wordpress, que consulta en la base de datos los contenidos que tiene que devolver y los presenta en los *templates* para presentar vistas o funcionalidades, a la par de esto, se ejecutan los *plugins* que son complementos para agregar funcionalidades adicionales a nuestros sitios. Sus características:

* Autoadministrable. Permite que el usuario administre sus contenidos sin tocar el código.
* Gestión de usuarios. Se pueden dividir los privilegios y los roles de los gestores de contenido.
* API REST. Permite generar una API que funcione mediante JS dentro del sitio o bien, utilizar wordpress como un backend y hacer el front con una tecnología distinta como React.
* Flexible. Permite personalizar su diseño (temas) y funcionalidades.
* Actualizaciones. Está en constante crecimiento.
* Comunidad. Cuenta con un gran equipo y comunidad que hace corregirlo y evolucionarlo.

# Temas
Son archivos que generar nuestras vistas en el navegador, esto depende de dos archivos:
* `index.php`: Todos los archivos dentro del template tomarán por defecto este archivo.
* `style.css`: Además de gestionar los diseños, tomará toda la data del template.
* `front-page.php`: Cargará la primera vista por defecto.
* `footer.php`: Cargará el footer con sus dependencias.
* `functions.php`: Permitirá agregar behaviors custom estendiendo el código de php.
* `header.php`: Cargará el header con sus dependencias.
* `404.php`: Cargará la vista de error para cuando se quiera acceder a rutas que no se puedan resolver.
* `page.php`: Pertenece a las páginas que generemos como administrador.
* `screenshot.png`: Será nuestra imagen de muestra en la selección del theme.
* `single.php`: Es una vista correspondiente a los post/entradas cuando no se especifica una vista concreta.

Toda esta estructura es la que da el dinamismo al diseño de nuestro sitio.

# Hooks
Los hooks, son funciones que nos permiten agregar código propio al código fuente de WP, dado que nosotros no podemos modificar directamente el código fuente, se dividen en dos tipos:
* Action. Nos permite agregar código o alguna funcionalidad muy específica. Se implementa con la función `add_action`, misma que se ejecuta en donde esté un `do_action`, ejemplo:
  ```php
    <?php
    function holaMundo() {
      echo "hola mundo";
    }
    add_action('wp_head', 'holaMundo');
    //add_action($hook, $function);
  ```
  En este caso estamos indicando el lugar donde se insertará y el nombre de la función.
* Filter. Hace lo mismo que el action, pero además recibe un argumento que se puede modificar y puede retornar, toma nuestro código con la función `add_filter`, la aplica en `apply_filter` y luego la retorna modificada:
  ```php
    <?php
    function upperTitle($title) {
      return strtoupper($title);
    }
    add_filter('the_title', 'upperTitle');
    //add_action($hook, $function);
  ```

# Manejo de librerías
Se pueden manejar distintos tipos de librerías, además, para cada tipo de archivo se usa una función diferente, ejemplo, para archivos css usaremos:
* `wp_register_style()`: registra la librería y tenerla a disposición como una dependencia pero no la ejecuta en el HTML. Recibe los siguientes argumentos:
  * `$handle string`: será el nombre que tenga nuestra librería para WP
  * `$src string|bool`: es el código fuente
  * `$deps array()array`: sirve para colocar los nombres de las librerías registradas
  * `$ver string|bool|null`: indica al navegador que genere el pedido de información si queremos hacer modificaciones
  * `$media all`: permite que el css se pueda ejecutar en todos lados o en una resolución/dispositivo específico
* `wp_enqueue_style()`: ejecuta directamente la librería pero también podemos programarla para llamar otras que tengamos registradas. Recibe los mismos argumentos que la anterior, solo que en lugar de solo registrar dependencias, las puede ejecutar.
Estas funciones se basan en distintas formas que tiene WP de tratar las librerías.

Para JS tenemos las siguientes librerías:
* `wp_register_script()`: registra la librería y tenerla a disposición como una dependencia pero no la ejecuta en el HTML. Recibe los siguientes argumentos:
  * `$handle string`: será el nombre que tenga nuestra librería para WP
  * `$src string|bool`: es el código fuente
  * `$deps array()array`: sirve para colocar los nombres de las librerías registradas
  * `$ver string|bool|null`: indica al navegador que genere el pedido de información si queremos hacer 
  modficaciones
  * `$in_footer bool`: determina si el script se ejecuta en el footer o en el header
* `wp_enqueue_script()`: ejecuta directamente la librería pero también podemos programarla para llamar otras que tengamos registradas. Recibe los mismos argumentos que la anterior, solo que en lugar de solo registrar dependencias, las puede ejecutar.

## Por qué manejar las librerías de forma dinámica?
La justificación de esto es que, podemos tener nuestros templates en distintos sitios con nuestras propias librerías, mismas que podríamos invocar en cada uno de ellos, además podemos controlar mediante php, el flujo de estas.

# xampp
Al instalar xampp, tenemos que considerar que para este caso, no es necesario instalar Filezilla, Mercury mail server, Tomcat, Webalizer ni Fake sendmail. La otra consideración importante es tener la carpeta de xampp en nuestra unidad prncipal.
Lo primero que haremos al instalar xampp, será inicializar los servicios de Apache y MySQL, con ello, veremos que en nuestro localhost, ya se verá el dashboard genérico de la aplicación.

# Instalación de WP
Iremos a wordpress.org a descargar la última versión disponible, nos descargará una carpeta llamada `wordpress`, misma cuyos archivos debemos copiar dentro de la carpeta xampp/htdocs, en el caso de mac: `/Users/{user}/.bitnami/stackman/machines/xampp/volumes/root/htdocs/{project-name}`, para acceder de manera más eficiente, seleccionamos explore dentro del menú *Volumes*.

El siguiente paso es crear nuestra BD, para ello, en xampp, iremos a MySQL/admin, esto se hace habilitando el puerto que vamos a utilizar y entrando a la ruta: `localhost:8080/phpmyadmin/`. Aquí veremos algunas Bds ya existentes, ara crear la nueva, damos click en la opción de menú izquierdo *Nueva* y en el campo que se despliega ponemos el nombre del proyecto, enseguida *Crear*, con ello ya veremos creada nuestra tabla. Una vez creada, buscamos en nuestra carpeta del código de wordpress, el archivo `wp-config-sample`, lo renombramos como `wp-config` y posteriormente lo abrimos con un editor de código, primeramente cambiaremos los primeros 3 define:
```php
define( 'DB_NAME', 'projectname' );

define( 'DB_USER', 'root' );

define( 'DB_PASSWORD', '' );
```

Los demás valores los dejamos por defecto de momento, más abajo en el archivo vemos los *salt keys*, que son métodos de seguridad que tiene wordpress para usuarios logueados, si los cambiamos, hacemos que si hay muchos usuarios logueados, salgan todos y se renueve la caché del sitio, este lo llenaremos con el los valores que nos arroje el siguiente [sitio](http://api.wordpress.org/secret-key/1.1/salt). De momento todos los demás valores quedarán por defecto, guardamos y vamos a `localhost:8080/projectname/`.
Al entrar, veremos la pantalla de instalación, donde nos pedirá nombrar el sitio y generar un usuario y contraseña, pulsamos instalar y una vez listo nos pedirá acceder.
Una vez ingresando, veremos el dashboard de admin, si visitamos el sitio veremos una plantilla por defecto.

# Dashboard de admin
Explorando el dashboard:
* Vista principal: nos sugiere acciones que podemos hacer, nos muestra brevemente nuestro contenido, la actividad reciente, un panel para notas privadas.
* Entradas: Nos permite añadir una nueva entrada, generar categorías y tags.
* Medios: Encontraremos todos los archivos multimedia que subamos, veremos que wp hace tres redimensiones para optimizar su impresión.
* Páginas: Son las rutas de nuestro sitio.
* Comentarios: En el caso de blogs, nos permitirá gestionar los comentarios: bloquear usuarios, aprobar comments o marcar como spam.
* Apariencia: En temas veremos algunos temas desarrollados por wp, en personalizar, cada tema nos permitirá cierta customización, widgets nos permitirá visualizar nuestros widgets actuales y administrarlos, gestionar los distintos menús que hay en el sitio o modificar el tema actual.
* Plugins. Permitirá administrar los plugins del sitio o modificarlos.

## Usuarios
Podemos agregar los usuarios que requiramos para administrar el sitio, donde lo importante es el perfil de cada uno:
* Administrador: administra todo en el sitio.
* Editor: maneja el contenido propio y de los demás pero no puede modificar el sitio.
* Autor: podrá crear contenido pero solo modificará el suyo.
* Colaborador: creará contenido pero no lo podrá publicar.
* Suscriptor: solo puede hacer comentarios.

> Se puede agregar permisos específicos a los usuarios mediante el uso de hooks, agregando una función en la ruta: `...\xampp\htdocs\projectname\wordpress\wp-includes\functions.php`. Más [info](https://wordpress.org/support/article/roles-and-capabilities/#administrator)

## Herramientas
* Salud del sitio: nos permitirá ver si hay mejoras a realizar y la información del sitio.
* Exportar los datos personales: nos permitirá gestionar peticiones de los usuarios de retirar sus datos personales.

## Ajustes
* Generales: veremos datos generales como el nombre del proyecto, descripción, la url, ajustes por defecto para usuarios, fechas, zona horaria, entre otros.
* Escritura: veremos configuraciones por defecto para las entradas así como la posibilidad de publicar mediante un email.
* Lectura: nos permitirá gestionar la paginación de las entradas y la visibilidad en buscadores.
* Comentarios: ajustes para los comentarios.
* Medios: permitirá configurar las resoluciones para los archivos multimedia.
* Enlaces permanentes: para configurar la forma en la que se verán las urls dentro del sitio.
* Privacidad: nos da la opción de generar una página que contendrá nuestra política de privacidad.

# Creando un theme
Para crear un tema nuevo, accederemos a `../htdocs/{project-name}/wp-comtent/themes`, ahí crearemos una carpeta nueva con el nombre de nuestro proyecto, una vez creada, la abriremos en nuestro editor de código y crearemos los siguientes archivos:
* `index.php`:  será la vista por defecto de cualquier vista que no esté asignada.
* `style.css`: en este archivo pasaremos todos los datos de nuestro tema, al inicio configuraremos los siguientes datos mediante un comentario:
```css
/*
  Theme name: Platzigifts
  Version: 1.0
  Description: sitio para catalogo de platzi
  Author: Uzi Rodriguez
  Author URI: [repo] 
  License: GNU General Public Licence v2 or later
  Licence URI: http://www.gnu.org/licenses/old-licenses/gpl-2.0.html
*/
```

Ahora ya tenemos configurado nuestro tema pero como aún no hemos cargado nada la vista es precaria.