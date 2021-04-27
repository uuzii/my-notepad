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

Ahora necesitaremos un archivo de código fuente. Para esto podríamos usar el [HelloWorld](https://nodejs.org/api/addons.html#addons_hello_world) de los addons de la página oficial de Node. Tomamos el código fuente en un archivo que nombraremos `hola.cc` y le haremos algunas modificaciones.

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

Posteriromente hacemos la configuración para poder mandar llamar este módulo. Lo haremos en un archivo que nombraremos `binding.gyp`:

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

