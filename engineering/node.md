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

# Callbacks
Ahora entraremos en el análisis de cómo gestionar procesos asíncronos. Consideremos el siguiente ejemplo, al ejecutar este código veremos que las impresiones en consola se ejecutan directamente una tras de otra en el orden en el que las escribimos.
```javascript
function soyAsincrona() {
  console.log('Hola soy una función asíncrona');
}

console.log('Iniciando proceso...');
soyAsincrona();
console.log('Terminando proceso...');
```

Sin embargo consideremos este otro ejemplo, en el que veremos que primero terminan las líneas principales y posteriormente se procesa lo que está dentro de la función `setTimeout`:
```javascript
function soyAsincrona() {
  setTimeout(() => {
    console.log('Hola soy una función asíncrona');
  }, 1000);
}

console.log('Iniciando proceso...');
soyAsincrona();
console.log('Terminando proceso...');
```

Ahora pensemos que requerimos que se ejecute otra función cuando termine nuestro proceso asíncrono. Para esto recordemos que las funciones son ciudadanos de primer nivel, quiere decir que podemos pasarlas como parámetro, de hecho esto ya lo estamos haciendo con el `console.log` que está dentro del `setTimeout`. A esta acción se le denomina *callback*. Hagámos unas modificaciones para ver cómo se usan las funciones como parámetro:
```javascript
function soyAsincrona(miCallback) {
  setTimeout(() => {
    console.log('Estoy siendo asíncrona');
    miCallback();
  }, 1000);
}

console.log('Iniciando proceso...');
soyAsincrona(function() {
  console.log('Terminando proceso...');
});
```

Agreguemos más parámetros para subir la complejidad, en este caso lo que haremos será *concatenar* una función asíncrona detrás de otra para que justo se ejecute después y además le transfiera una variable:
```javascript
function hola(nombre, miCallback) {
  setTimeout(() => {
    console.log('Hola ' + nombre);
    miCallback(nombre);
  }, 1000);
}

function adios(nombre, otroCallback) {
  setTimeout(() => {
    console.log('Adios ' + nombre);
    otroCallback();
  }, 1000);
}

console.log('Iniciando proceso...');
hola('Uzi', function(nombre) {
  adios(nombre, function() {
    console.log('Terminando proceso...');
  });
});
```

## Callback hell
Debemos tener cuidado al manejar estas funciones asíncronas, pues se puede volver muy intricada su concatenación. imaginemos para el ejemplo anterior, que agragamos una tercera función y que la mandaremos a llamar de manera intermedia:
```javascript
function hola(nombre, miCallback) {
  setTimeout(() => {
    console.log('Hola ' + nombre);
    miCallback(nombre);
  }, 1500);
}

function hablar(callbackHablar) {
  setTimeout(() => {
    console.log('bla bla bla bla...');
    callbackHablar();
  }, 1500);
}

function adios(nombre, otroCallback) {
  setTimeout(() => {
    console.log('Adios ' + nombre);
    otroCallback();
  }, 1000);
}

console.log('Iniciando proceso...');
hola('Uzi', function(nombre) {
  hablar(function() {
    hablar(function() {
      adios(nombre, function() {
        console.log('Terminando proceso...');
      });
    });
  });
});
```

Como podemos notar, nuestro código se empieza a acrecentar pero sobre todo comienza a verse muy horizontal, para empezar a solventar este código intrincado, un primer paso sería resumir nuestras funciones `hablar` en una función recursiva para configurar diretamente el número de veces que se ejecute:
```javascript
function hola(nombre, miCallback) {
  setTimeout(() => {
    console.log('Hola ' + nombre);
    miCallback(nombre);
  }, 1500);
}

function hablar(callbackHablar) {
  setTimeout(() => {
    console.log('bla bla bla bla...');
    callbackHablar();
  }, 1500);
}

function conversacion(nombre, veces, callback) {
  if(veces > 0) {
    hablar(function() {
      conversacion(nombre, --veces, callback);
    });
  } else {
    adios(nombre, callback);
  }
  
}

function adios(nombre, otroCallback) {
  setTimeout(() => {
    console.log('Adios ' + nombre);
    otroCallback();
  }, 1000);
}

console.log('Iniciando proceso...');
hola('Uzi', function(nombre) {
  conversacion(nombre, 3, function() {
    console.log('proceso terminado');
  });
});
```

# Promesas
Las promesas nos permiten gestionar nuestras funciones asíncronas de una manera más limpia (sugar syntax). La diferencia con el simple uso de callbacks es que éstas manejan diferentes estados como *resolve*, *pending*, *reject*. Refactorizando las funciones de los ejemplos anteriores tendríamos lo siguiente:
```javascript
function hola(nombre) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('Hola ' + nombre);
      resolve(nombre);
    }, 1500);
  });
  
}

function hablar(nombre) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('bla bla bla bla...');
      resolve(nombre);
    }, 1500);
  });
}

function adios(nombre) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('Adios ' + nombre);
      resolve();
    }, 1000);
  });
}

console.log('Iniciando el proceso');
hola('Uzi')
  .then(hablar)
  .then(adios)
  .then(() => {
    console.log('Terminado el proceso');
  })
```

En este ejemplo vemos que el promise recibe dos funciones genéricas: `resolve` y `reject` en lugar de callbacks, la primera sirve para enviar parámetros a toda la cadena de nuestras promesas y la segunda sirve para indicar a la cadena de promesas cuando no se ha podidido ejecutar el proceso asíncrono. También vemos que la concatenación de dicha cadena es mucho más legible y no nos exige el pase de parámetros. ¿Cómo funciona el reject? imaginemos que configuramos el promise de nuestra función `hablar` para que ejecute un reject, entonces basta con colocar un `catch`al final de nuestra cadena de promesas para que ese error sea notificado:
```javascript
...
function hablar(nombre) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('bla bla bla bla...');
      //resolve(nombre);
      reject('Hay un error');
    }, 1500);
  });
}
...

console.log('Iniciando el proceso');
hola('Uzi')
  .then(hablar)
  .then(adios)
  .then(() => {
    console.log('Terminado el proceso');
  })
  .catch(error => {
    console.error('ha habido un error');
    console.error(error);
  });
```

Dado este ejemplo, podemos ver la importancia de utilizar siempre el `catch` pues no sabemos cuándo puede fallar uno de nuestros procesos, cualquier proceso puede fallar.

# Async-await
Async-await es una nueva sintaxis que nos permite definir cualquier función como asíncrona para que dentrode ella se puedan crear fácilmente procesos asíncronos de manera sencilla. Técnicamente sigue funcionando de la misma manera el asincronismo, solo es sugar syntax. Refactoricemos las funciones del ejemplo anterior:
```javascript
async function hola(nombre) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('Hola ' + nombre);
      resolve(nombre);
    }, 1500);
  });
  
}

async function hablar(nombre) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('bla bla bla bla...');
      resolve(nombre);
    }, 1500);
  });
}

async function adios(nombre) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log('Adios ' + nombre);
      resolve();
    }, 1000);
  });
}

async function main() {
  let nombre = await hola('Uzi');
  await hablar();
  await hablar();
  await hablar();
  await adios(nombre);
  console.log('Terminando proceso...');
}

console.log('Iniciando proceso...');
main();
console.log('Segunda instrucción');
```

Aquí vemos como podemos guardar el `resolve` de una promesa dentro de una variable para poder utilizar el valor obtenido en cualquier parte subsecuente de nuestro código con el simple hecho de colocar la palabra reservada `await`, el único requisito es que la función se anteceda también con la palabra reservada `async`.

# Core modules
Con la finalidad de entender los módulos que nos brinda node, es importante recurrir a la [referencia](https://nodejs.org/es/docs/) de la api, donde podremos encontrar la documentación de los módulos que existen.

## Globales
Son módulos que ya vienen incluídos en el core de node, sin darnos cuenta hemos usado `console`, `setTimeout`, no han salido de la nada sino de estos módulos.

Si imprimimos en una terminal la palabra `global` podríamos ver los módulos del core, ejemplo clearInterval, setInterval, etc:
```javascript
console.log(global);
```

Por ejemplo, las funciones mencionadas se usan mucho cuando se está reintentando acceder a una BD después de cierto número de fallos:
```javascript
let i = 0;
let interval = setInterval(function() {
  console.log('hola');
  if(i === 3) {
    clearInterval(interval);
  }
  i++
});
```

En los globales encontramos muchas más funciones como `require`, obtener mucha información del `process`, tenemos acceso al path en el que nos encontramos como `__dirname` o `__filename` para leer el nombre del archivo. Debemos evitar utilizar variables globales, pero también podríamos hacerlas con:
```javascript
global.miVariable = 'value';
console.log(miVariable);
```

## File system
Este será un módulo muy utilizado pues podremos analizar archivos, generarlos, eliminarlos, etc. Lo que tenemos que hacer para implementarlo es requerir filesystem con:
```javascript
const fs = require('fs');
```

Para leer un archivo, por ejemplo, podríamos creat un archivo llamado `file.txt` a la par de nuestor js e implementar lo siguiente:
```javascript
const fs = require('fs');

function read(path, cb) {
  fs.readFile(path, (err, data) => {
    console.log(data.toString())
  })
}

read(__dirname + '/file.txt');
```

Para generar un archivo:
```javascript
function write(path, content, cb) {
  fs.writeFile(path, content, function(err) {
    if(err) {
      console.error('No he podido escribirlo', err);
    } else {
      console.log('Se ha escrio correctamente');
    }
  })
}

write(__dirname + '/another-file.txt', 'im that file', console.log);
```

Para borrar un archivo:
```javascript
function deleteSomeFile(path, cb) {
  fs.unlink(path, cb);
}

deleteSomeFile(__dirname + '/another-file.txt', console.log);
```

## Console
El módulo `console` es uno de los más usados pero muchas veces no se conocen todas sus funcionalidades, veremos algunas:
```javascript
// imprime un mensaje informativo
console.info('another thing');
// imprime un warning
console.warn('another thing');
// imprime error
console.error('some error');

var table = [
  { a: 1, b: 2 },
  { a: 3, b: 4 }
]
// imprime un objeto con la información en una tabla
console.table(table);
// imprime un grupo de mensajes con indentación
console.group('conversation');
console.log('Hola');
console.log('bla bla bla');
console.log('Adiós');
console.groupEnd('conversation');

// Imprime un mensaje + las veces que se ha impreso y luego limpia el contador
console.count('veces');
console.count('veces');
console.count('veces');
console.countReset('veces');
console.count('veces');
```

## Errores
Es importante que siempre tengamos prevención para los errores que pudieran saltar en nuestro código. Consideremos la siguiente función en la que ocurrirá un error:
```javascript
function itBreaks() {
  return 3 + z;
}

itBreaks();
```

Para prevenir este error, envolvemos la ejecución de la función dentro de un *try catch*, esto es importante ya que en JS los errores pueden detener nuestro único hilo y así detener todo nuestro programa, pero con try catch podemos hacer un *handle* y reintentar o al menos imprimir un mensaje:
```javascript
try {
  itBreaks();
} catch(err) {
  console.error('Something is broken...', err);
}
```

Ahora, consideremos que los errores se propagan hasta la primera función en la que se han originado, esto quiere decir que si en dicha función se quiere hacer un tratamiento, por ejemplo, podemos tener acceso al mismo para modificar el comportamiento de nuestro programa:
```javascript
function anotherFunction() {
  return itBreaks();
}

function itBreaks() {
  return 3 + z;
}

try {
  anotherFunction();
} catch(err) {
  console.error('Something is broken...', err);
}
```

El ejemplo anterior, como quiera nos imprime el error puntual que ocurrió en una función interna y además nos indicará una traza del mismo.

Para funciones asíncronas, consideremos lo siguiente:
```javascript
function itBreaksAsynchronus() {
  setTimeout(() => {
    return 3 + z;
  }, 1000);
}

try {
  itBreaksAsynchronus();
} catch(err) {
  console.error('Something is broken...', err);
}
```

En este ejemplo, se romperá toda nuestra aplicación, pues no se tiene prevenida la excepción dentro del callback, sino en el hilo principal y hasta él nunca llegó, por lo cuál, si queremos provenir un error desde la función asíncrona, podemos enviar un callback y generar el bloque *try catch* dentro del propio proceso asíncrono:
```javascript
function itBreaksAsynchronus(cb) {
  setTimeout(() => {
    try {
      return 3 + z;
    } catch(err) {
      console.error('Error en proceso asíncrono');
      cb(err)
    }
  }, 1000);
}

try {
  itBreaksAsynchronus(function(err) {
    console.error(err.message)
  });
} catch(err) {
  console.error('Something is broken...', err);
}
```

## Procesos hijo
Node.js nos permite ejecutar sus propios procesos y además ejecutar otros procesos hijo, pueden ser otros de node, de python, variables del sistema, scripts, etc. Consideremos el siguiente fragmento:
```javascript
const { exec } = require('child_process');

exec('ls -la', (err, stdout, sterr) => {
  if(err) {
    console.error(err);
    return false;
  }

  console.log(stdout);
});
```

El ejemplo anteriro nos permite listar todos los archivos en el directorio actual, de la misma forma podríamos ejecutar otro de nuestros módulos o el proceso que queramos del sistema.

Si queremos solo ejecutar un proceso nuevo y trackearlo, escribimos lo siguiente:
```javascript
const { spawn } = require('child_process');
// se separan los parámetros
let process = spawn('ls', ['-la']);
```

Ahora podemos saber cosas acerca del proceso, veamos:
```javascript
//const { exec } = require('child_process');
const { spawn } = require('child_process');

let process = spawn('ls', ['-la']);

// imprimir todo el proceso
console.log(process);

// Imprime el  id del proceso
console.log(process.pid);

// imprime si el proceso está conectado
console.log(process.connected);

// imprime el flujo de datos del proceso
process.stdout.on('data', function(info) {
  console.log(info.toString())
});

process.on('exit', function() {
  console.log('el proceso terminó');
});
```

## Módulos nativos en C++ desde JavaScrip
Veremos un ejemplo de cómo compilar un módulo externo de C++, esto será útil cuando encontremos código útil propio o de terceros y lo queramos usar en nuestras aplicaciones.

Lo primero que tendremos que hacer es instalar node gyp:
```bash
npm i -g node-gyp
```

Ahora necesitaremos un archivo de código fuente. Para esto podríamos usar el [HelloWorld](https://nodejs.org/api/addons.html#addons_hello_world) de los addons de la página oficial de Node. Tomamos el código fuente en un archivo que nombraremos `hola.cc` y le haremos algunas modificaciones:

```c++
// hola.cc
#include <node.h>

namespace demo {

using v8::FunctionCallbackInfo;
using v8::Isolate;
using v8::Local;
using v8::Object;
using v8::String;
using v8::Value;

void Method(const FunctionCallbackInfo<Value>& args) {
  Isolate* isolate = args.GetIsolate();
  args.GetReturnValue().Set(String::NewFromUtf8(
      isolate, "mundo").ToLocalChecked());
}

void Initialize(Local<Object> exports) {
  NODE_SET_METHOD(exports, "hola", Method);
}

NODE_MODULE(NODE_GYP_MODULE_NAME, Initialize)

}  // namespace demo
```

Posteriromente hacemos la configuración para poder mandar llamar este módulo. Lo haremos en un archivo que nombraremos `binding.gyp`, en el cual indicaremos mediante un json el módulo a compilar, cómo se va a llamar y la ruta:

```gyp
{
  "targets": [
    {
      "target_name": "addon",
      "sources": [ "hola.cc" ]
    }
  ] 
}
```

Ahora ejecutamos:
```bash
node -gyp configure
```

Al finalizar la ejecución veremos que nos genera una carpeta `/build` en la cuál estará todo el código que necesita node para poder utilizar nuestro módulo. Para construirlo, ejecutamos:
```bash
node-gyp build
```

Al finalizar la ejecución ya estará listo nuestro módulo, podemos probarlo en un `index.js` genérico:
```javascript
//index.js
const miAddon = require('./build/Release/addon');
console.log(miAddon.hola());
```

Lo anterior considerando que hemos configurado el nombre de nuestro módulo como `addon`. Veremos imprimirse la palabra "mundo".

## HTTP
Este módulo nos permitirá conectarnos a un servidor o crear uno desde node sin pasar por ningún otro sitio. Para empezar, generaremos un archivo `http.js`:
```javascript
//http.js
const http = require('http');

http.createServer(function(req, res) {
  console.log('nueva petición');
  console.log(req.url);
  
  res.end();
}).listen(3000);

console.log('Escuchando http en el puerto 3000');
```

Hasta aquí hemos creado un servidor que está escuchando el puerto 3000 y nos avisará cada que haya una nueva petición así como la url de donde proviene la misma.
Si queremos escribir algo a la respuesta, hacemos lo siguiente:
```javascript
// http.js
  http.createServer(function(req, res) {
    console.log('nueva petición');
    console.log(req.url);
    
    // escribir respuesta
    res.write('Hola ya sé usar http');

    res.end();
  }).listen(3000);
```

Para enviar headers hacemos lo siguiente:
```javascript
  res.writeHead(201, {'Content-Type': 'text/plain'})
```

Con ello veríamos el status 201 y un nuevo content type. Para hacer nuestro código más funcional, podríamos generar un *router*, cuya función sería enviar respuestas distintas para cada url, para ello refactorizaremos nuestro código separando primeramente el callback:
```javascript
const http = require('http');

http.createServer(router).listen(3000);

function router(req, res) {
  console.log('nueva petición');
  console.log(req.url);

  switch(req.url) {
    case '/hola':
      res.write('Hola, qué tal');
      res.end();
      break;
    default:
      res.write('Error 404: no sé lo que quieres');
      res.end();
  }
}

console.log('Escuchando http en el puerto 3000');
```

## OS
Con este módulo podremos acceder a información que normalmente solo se obtiene en lenguajes de muy bajo nivel desde Javascript. Para empezar crearemos un archivo `os.js` donde podremos usar distintas funciones del módulo:
```javascript
const os = require('os');
console.log(os.arch()); // arquitectura
console.log(os.platform()); // sistema operativo
console.log(os.cpus()); // núcleos
console.log(os.constants); // errores y señales del sistema
console.log(os.freemem()); // bytes disponibles
console.log(os.totalmem()); // bytes totales
console.log(os.homedir()); // directorio raiz
console.log(os.tmpdir()); // directorio temporal
console.log(os.hostname()); // nombre del host de la máquina
console.log(os.networkInterfaces()); // interfaces de red
```

## Process
Ahora veremos cómo podemos acceder a nuestro proceso de node para ver su ejecución. Los primeros indicadores que podemos ver del proceso son al salir o antes de salir:
```javascript
process.on('beforeExit', () => {
  console.log('El proceso va a terminar');
});

process.on('exit', () => {
  console.log('El proceso acabó');
});
```

Consideremos que, una vez que se ha ejecutado el exit, ya se ha cerrado el evenloop y ya no se podría ejecutar nada.

> Nótese que no es necesario requerir `process`.

Si queremos recoger algún error que no haya sido tomado usamos:
```javascript
process.on('uncaughtException', (err, origin) => {
  console.log('Hemos olvidado capturar el error', err);
});

functionQueNoExiste()
console.log('Esto si el error no se recoje, no aparece');
```

Así como estas, hay muchas señales del proceso que podemos escuchar para detonar acciones precisas conforme a nuestras necesidades.

# Paquetes y módulos externos
Hasta ahora hemos revisado el uso de módulos propios de node, pero también existen módulos y paquetes de terceros, para utilizar estos paquetes, el gestor más famoso es `npm`. Para encontrar estos paquetes, navegaremos a través de la web de [npm](https://www.npmjs.com). Ejemplo, podríamos utilizar el paquete [is-odd](https://www.npmjs.com/package/is-odd) para determinar si el número es par o impar, aunque esta feature pareciera simple, el paquete incluye casos de uso que posiblemente nosotros no hemos contemplado y así nos ahorraría programarlos. Cuando queramos implementar alguno de estos paquetes, diremos que nos hemos añadido una *depedencia*, pues en cierto grado dependemos de ese código para que funcione nuestra aplicación, todos estos paquetes a su vez tienen dependencias pero siempre es importante revisar su estabilidad y actualizaciones para saber si es conveniente usarlo. Para instalar un paquete usamos el comando:

```bash
npm install [package-name]
```

Notamos que al instalar un paquete nos genera un archivo llamado `package-lock.json` y una carpeta llamada `node_modules`, pero ¿qué es esto?. Vayamos un paso atrás y consideremos que no están estos archivos, si estando en nuestra carpeta raíz ejecutamos el comando `npm init`, veremos que también se genera el archivo `package.json`, el `package-lock.json` y la carpeta `node_modules` en el primer archivo se enlistan todas las dependencias que usará nuestro proyecto, mismas cuyo código se guardará en `node_modules` solo con la palabra `require([package-name])`.

## Construyendo módulos
Si nosotros queremos construir nuestro propio módulo, seguiremos los siguientes pasos: generamos una carpeta, llamémosla `modulo` con un archivo `index.js` y un archivo `modulo.js`, cuyo contenido será:
```javascript
// modulo.js
function saludar() {
  console.log('Hola mundo');
}

module.exports = {
  saludar,
  prop: 'info'
};
```

Ahora en nuestro index:
```javascript
// index.js
const modulo = require('./modulo');
modulo.saludar(); // 'Hola mundo'
console.log(modulo.prop); // 'info'
```

Con esto podríamos ver en nuestro index tanto funciones como información que está dentro de un módulo. Ésta es la sintáxis de `require`, pero existe una nueva sintáxis en ES6, donde nuestro módulo se exportaría de la siguiente manera:
```javascript
// modulo.js
function saludar() {
  console.log('Hola mundo');
}

export default {
  saludar,
  prop: 'info
}
```

Y en nuestro index:
```javascript
// index.js
import modulo from './modulo.js'
modulo.saludar(); // 'Hola mundo'
console.log(modulo.prop); // 'info'
```

Con lo cual obtendremos el mismo resultado, si bien esta feature era experimental hace algún tiempo, ahora ya es estándar y podemos usarla en nuestro módulos para producción.

# Algunos módulos útiles
Los siguientes módulos nos permitirán trabajar con fechas, imágenes y cifrados:

## bcrypt
Este módulo nos permite cifrar, para empezar a usarlo, inicializamos nuestro proyecto:
```bash
npm init -y
```

Ahora instalamos `bcrypt` mediante:
```bash
npm i bcrypt
```

Para implementar bcrypt podemos hacerlo con la siguiente función:
```javascript
const bcrypt = require('bcrypt');

const password = '1234segura!';

/**
 * con la función hash cifraremos el dato
 * @param {Any} password dato a encriptar
 * @param {Number} 5 número de veces que se aplica el hash
 * @param {Function} callaback
 */
bcrypt.hash(password, 5, function(err, hash) {
  console.log(hash)
});
```

En esta última ejecución, veremos la información acerca del algoritmo del hash y el número de veces + nuestro hash que será diferente cada vez.

Si queremos comprobar que el hash que hemos generado proviene del texto inicial, implementamos la función `compare` dentro del callback;
```javascript
bcrypt.hash(password, 5, function(err, hash) {
  console.log(hash);
  bcrypt.compare(password, hash, function(err, res) {
    console.log(res);
  })
});
```

## moment
Para manejar horas y fechas, la librería por defecto es `moment.js`, para instalarlo, usamos
```bash
npm i moment
```

Algunos ejemplos básicos de uso son los siguientes:
```javascript
const moment = require('moment');

let ahora = moment();
// Imrimir hora, fecha, zona horaria actuales
console.log(ahora.toString());

// imprimir hora y fecha con cierto formato
console.log(ahora.format('YY/MM/DD - HH:mm'));
```

Pero además de estos pequeños ejemplos, moment tiene muchísimas opciones para trabajar con fechas y horas.

## sharp
Sharp nos permite modificar imágenes, este sería un ejemplo en el que redimensionaríamos una imagen y la pasaríamos a blanco y negro:
```javascript
const sharp = require('sharp');

sharp('image.png')
  .resize(80)
  .grayscale()
  .toFile('resized.png')
```