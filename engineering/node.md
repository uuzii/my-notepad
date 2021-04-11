#  ¿Qué es Node.js?
Es un entorno de ejecución de JavaScript fuera del navegador. Fue creado en 2009 y está orientado a servidores. El que esté fuera del navegaddor significa que se puede ejecutar en cualquier computadora, generalmente lo usaremos para servidores.

## Características de Node
* **Es concurrente:** sus entradas y salidas son asíncronas, se ejecuta un proceso por cada núcleo del procesador.
* **Corre sobre el motor V8:** el código se convierte a código máquina de manera muy óptima ya que está hecho en C++ (y es open source).
* **Todo funciona en base a módulos:** son piezas de código muy pequeñas que podemos recoger de distintos sitios bien organizados.
* **Orientado a eventos:** hay un bucle de eventos que está ejecutándose constantemente y de vez en cuando se despacharán eventos que nos permitan ejecutar otras partes del código, es decir que programaremos de manera reactiva.

# Eventloop
El eventloop es un bucle que se está ejecutando todo el tiempo, este está gestionado automáticamente por un proceso que permite que todo lo que se reciba funcione de maera asíncrona, es decir, que no se bloqueará el bucle, sino que delegará el procesamiento al *Thread pool* para procesar operaciones lentas y el eventloop seguirá recibiendo eventos. El *thread pool* levantará un hilo para cada tarea que requiera y cuando estén listas, su resolución se encolará para poder presentarse de nuevo al eventloop.

![Eventloop](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/eventloop-thread.jpg?raw=true)

# Monohilo: implicaciones en diseño y seguridad
El hecho de que solo exista un hilo puede ser tanto ventaja como desventaja si no se entiende su funcionamiento.

Para empezar a entenderlo, creamos un archivo con la extensión `js`:
```javascript
// monohilo.js
console.log('Hola mundo');
```

Para ejecutarlo, ejecutamos en una terminal:
```bash
node monohilo.js
```

Veremos que se está ejecutando nuestro console log, pero aquí consideremos que está sucediendo lo siguiente:
* Se abre un proceso de node: interpreta todo el archivo, lo convierte a código máquina y lo prepara para ejecutarse
* Se ejecuta en la terminal
* Termina el proceso

Pero consideremos la problemática que se puede presentar, imaginemos el siguiente código:
```javascript
let i = 0;
setInterval(function() {
  console.log(i);

  if(i === 5) {
    var a = 3 + z;
  }
}, 1000);
```

En este ejemplo, la sentencia que está dentro del `if`, genera un error, el compilador no lo notará, pero en tiempo de ejecución, habrá un cloqueo seguro, es por ello que debemos tener prevenidos siempre los errores a lo largo de nuestro código, sobre todo si está en producción.

Consideremos otro ejemplo:
```javascript
console.log('hola mundo')

let i = 0;
setInterval(function() {
  console.log(i);
}, 1000);

console.log('segunda instrucción')
```

A la salida de este programa, veremos que se imprime *hola mundo* e inmediatamente después *segunda instrucción*, esto es porque los console logs son funciones que están sucediendo en un hilo y el proceso `setInterval` entra después porque su callback fue encolado pues fue procesado en otro hilo.

# Variables de entorno
Las variables de entorno, nos servirán para introducir información externa a nuestros programas. Para esto, accederemos al proceso mediante `process` y posteriormente al environment mediante la propiedad `env`:
```javascript
// entorno.js
let name = process.env.NAME || 'Sin nombre';
console.log('Hola ' + name);
```

Para ejecutarlo:
```bash
NOMBRE=Uzi node entorno.js
```

Veremos a la salida que la variable se introduce a nuestro programa, de esta manera podemos introducir cualquier número de variables a nuestros programas (nombradas con mayúsculas por estándar).

# Nodemon y PM2
Nodemon es una herramienta que utilzaremos desde la línea de comands para ejecutar nuestro código nuevamente cada vez que le hagamos un cambio. Para instalarlo usamos:
```bash
npm install -g nodemon
```

De esta manera podemos aumentar nuestra productividad al estar trabajando con node, esto es en desarrollo, pero en producción podemos usar PM2, se instala con:
```bash
npm install -g pm2
```

Este paquete nos permitirá ejecutar nuestra aplicación en un cluster, hacer watch and reload, monitorear la aplicación, imprimir los logs en un archivo, volver a ejecutar la aplicación si falla, entre otras utilidades.