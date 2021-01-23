# Algunos conceptos

Para inicializar un poryecto con todas las banderas en `y` usamos `npm init -y`
Una herramienta que nos puede ayudar para servir nuetros proyectos vanilla es **live-server**

# ¿Cómo llega un script al navegador?
El Document Object Model (DOM), es una representación que hace el navegador del documento HTML. Cuando el navegador recibe el documento HTML que nosotros escribimos, genera un árbol, que es una estructura fácil de procesar, éste es el DOM, el `DOMContentLoaded`, que es un punto en el que ya está listo el DOM para ser utilizado. Esto se cumple con todas las etiquetas de HTML, excepto la etiqueta `script`. Cuando encuentra una etiqueta `script`, el navegador se detendrá a ejecutar el código embebido dentro de ella para poder procesar el código que viene después. En el siguiente ejemplo, veremos un error dado que primero se ubica el script y luego el `form`, que no ha sido procesado.

```html
<body>
  <script>
    let el = document.querySelector('#login')
    el.addEventListener('submit', function(e) { ... }) // typeError
  </script>
  <form id="login" action="/login" method="POST">
    <!-- .. -->
  </form>
</body>
```

Esto mismo ocurre con scripts externos implementados mediante el atributo `src`.

## Aync scripts
Actualmente existe un atributo denominado `async`, que permite que nuestros scripts se procesen de manera *asíncrona*, sin detener el procesamiento del HTML sino hasta que éstos se resuelvan, pero por eso mismo, solo hay que usarlos con scripts que nos son fundamentales para la página, generalmente third-party scripts o analytics:

```html
<head>
  <script async src="[url]"></script>
</head>
```

### ¿Qué pasa si hay dos scripts asíncronos?
Ambas peticiones saldrán, pero no sabemos en qué momento se resolverán y detendrán momentáneamente nuestro procesamiento del HTML.

## Defer scripts
Defer es otro atributo, que difiere la ejecución del javascript hasta el final del HTML. El *fetching* se ejcuta pero no se procesará el código embebido sino hasta el final del procesamiento del HTML.

```html
<body>
  <script defer src="[url]"></script>
  <!-- a bunch of HTML -->
</body>
```

# Scope
Es el ámbito de una variable, o dicho de otro modo: el tiempo de vida que tiene una variable. En JS siempre existieron dificultades para el manejo del scope de las variables, pero en la actualidad existen herramientas de declaración como `let`y `cost` que nos facilitan su manejo. Consideremos algunas aseveraciones:

* Toda variable declarada fuera del ámbito de una función o de un bloque, pertenecerá al scope global, que es el objeto `window`
* Toda librería o script externo importado directamente en la página también estará disponible en el `window`
* Siempre que aparezca la palabra `var`, se creará una variable nueva con el nombre valor asignado (puede pisar a una homónima).
* Dentro de cada llamado a una función, se opera un scope único.
* `let` y `const` operan sobre el *block scope*, esto quiere decir que dentro del bloque, la variable será *recordada* siempre.

## Module scope
Es un caso especial del scope, en el que truncamos el acceso del scope gobal a un scope más particular:

```html
  <script type="module" src="/assets/index.js"></script>
```

En este ejemplo, el scope global, no podrá tener acceso a las variables que existan dentro del módulo index.js

En resumen, existen cuatro tipos de scope en JS:

* **Global**. Que es el `window`.
* **Function**. Que es el que existe dentro un llamado a una función.
* **Block**. Que se determina por un bloque.
* **Module**. Que se delimita por el atributo type module.

# Closures
Un closure, es una función que tiene acceso al scope en el que fue declarada, veamos un ejemplo con una IFEE:

```javascript
(function() {
  let color = 'green'
  function printColor() {
    console.log(color)
  }
  printColor() // green
})()
```

En este ejemplo, vemos que, aunque la función `printcolor`, en teoría no dbería tener acceso a la variable `color`, sí lo tiene, pues tanto la función como la variable comparten el *function scope*.

Toda vez que una función sea creada dentro de otra función, la primera tiene acceso a todo el scope de la segunda:

```javascript
function makecolorPrinter(color) {
  let colorMessage = `The color is ${color}`
  return function() {
    console.log(colorMessage)
  }
}
let colorGreenPrinter = makeColorPrinter('green')
console.log(colorGreenPrinter())
```

Notemos que en este caso, se está retornando una función y ocurre el mismo efecto de acceso al scope *padre*, es decir, esto también es un closure.

Los closures nos permiten emular un feature que el lenguaje no tiene, que son las **variables privadas*, veamos un ejemplo:

```javascript
function makeCounter(n) {
  let count = n
  return {
    increase: function() { count = count +1 },
    decrease: function() { count = count -1 },
    getCount:  function() { return count }
  }
}
let counter = makeCounter(7) // counter.count no existe fuera de este contexto = variable privada
console.log('The count is ' + makeCounter.getCount()) // 7
```

Nótese que desde el ámbito externo no se puede afectar a la variable interna llamada `count`, para hacer esto, se tendría que operar por medio de las funciones que sí están expuestas.

# this
En la mayoría de los lengajes de programación, `this`, hace referencia a la instancia de la clase que se está utilizando, en el caso de JavaScript, sabemos que no existen las clases, generalmente nos referiremos a instancias pero de protoypes, realmente puede tener muchas variantes, veamos algunas de ellas:

## this en el contexto global
En el contexto global, `this` hace referencia directamente al `window`
```javascript
console.log(this) // window
```

## this en el contexto de una función
En el contexto de una función, cuando `this` está siendo llamada directamente desde el `window`, será `window`, sí y solo sí no estamos utilizando `strict mode`
```javascript
function whoIsThis() {
  return this
}
console.log(this) // window
```

En strict mode, `this` será undefined, podemos usarlo para solventar algunos errores que nos puede ocasionar el ejemplo anterior.
```javascript
'use strict'
function whoIsThis() {
  return this
}
console.log(this) // undefined
```

## this en el contexto de un objeto
En un objeto, `this` hace referencia a la instancia del propio objeto
```javascript
const person = {
  name: 'uzi',
  greeting: function() {
    console.log(`Hola soy ${this.name}`) // Hola soy uzi
  }
}
person.greeting()
```

## this en el contexto de una clase
Funciona de manera similar que en las clases, pero tomemos en cuenta que en ambos casos, this hace referencia a la instancia, no al objeto prototipal:
```javascript
function Person(name) {
  this.name = name
}
Person.protoype.greeting = function() {
  console.log(`Hola me llamo ${this.name}`)
}

const uzi = new Person('Uzi')
uzi.greeting() // Hola me llamo Uzi
```

# Establecer el this del llamado a una función
No es posible asignar directamente el `this` en ningún contexto, para esto existen las funciones `bind`, `call` y `apply`, que nos permitirán pasarle un this diferente al contexto de ejecución de una función. En este caso, usaremos `call` y como primer parámetro el `this` (contexto que queremos pasar) para hacer el llamado inmediato de una función (greeting) que originalmente no debería tener el contexto de un objeto ajeno (person):
```javascript
function greeting() {
  console.log(`Hola, soy ${this.name} ${this.lastName}`)
}

const person = {
  name: 'Uzi',
  lastName: 'Rodriguez'
}
greeting.call(person) // Hola, soy Uzi Rodriguez
```

Esta función, también pueden aceptar más parámetros aparte de `this`:
```javascript
function walk(metros, direccion) {
  console.log(`${this.name} camina ${metros} hacia el ${direccion}`)
}
walk.call(this, 400, 'norte') // Uzi camina 400 metros hacia el norte
```

La función `apply`cumple la misma función, pero recibe los argumentos de la función de una manera distinta, los recibe en forma de array:
```javascript
function walk(metros, direccion) {
  console.log(`${this.name} camina ${metros} hacia el ${direccion}`)
}
const arguments = [400, 'norte']
walk.apply(this, arguments) // Uzi camina 400 metros hacia el norte
```

La función `bind`, por su lado, nos permite crear una función nueva con otro contexto, pero no la llama:
```javascript
const anotherPerson = {
  name: 'Jonadab',
  lastName: 'Alcantara'
}
const bindedGreeting = greeting.bind(anotherPerson)
bindedGreeting() // Hola, soy Jonadab Alcantara
```

Tomando como referencia el ejemplo anterior, si queremos pasar argumentos al llamado de una función que ya fue creada y atada a otro contexto, mandamos los argumentos:
```javascript
const bindedWalk = walk.bind(anotherPerson)
bindedWalk(400, 'norte') // Jonadab camina 400 metros hacia el norte
```

O se pueden almacenar al momento de generar la función:
```javascript
const bindedWalk = walk.bind(anotherPerson, 400, 'norte')
bindedWalk() // Jonadab camina 400 metros hacia el norte
```

O también se puede mezclar los últimos dos ejemplos, estableciendo algunos valores y pasando otros en la ejecución:
```javascript
const bindedWalk = walk.bind(anotherPerson, 400)
bindedWalk('norte') // Jonadab camina 400 metros hacia el norte
```

# Prototypes
Es un concepto casi único de JavaScript. En otros lengiajes orientados a Objetos, se crean solo instancias de clases, en JS no existen las clases, todo son objetos, sin embargo podemos generar herencia a través de lo que se denomina un *prototipo*.

Podemos enfrentarnos a la siguiente situación mientras estamos instanciando objetos:
```javascript
function Hero(name) {
  const hero = {
    name: name
  }
  hero.saludar = function() {
    console.log(`hola soy ${this.name}`)
  }
  return hero
}
const zelda = Hero('Zelda')
const link = Hero('Link')
```

Nótese que cada que queremos instanciar, estamos generando una función dentro del scope de cada objeto para eficientar esto, podríamos usar un método genérico y hacer referencia a él:
```javascript
const heroMethods = {
  saludar: function() {
    console.log(`Hola soy ${this.name}`)
  }
}
function Hero(name) {
  const hero = {
    name: name
  }
  hero.saludar = heroMethods.saludar
  return hero
}
const zelda = Hero('Zelda')
const link = Hero('Link')
```

Hay una forma más reducida de hacer esto, y es usando la función `Object.create()`:
```javascript
const heroMethods = {
  saludar: function() {
    console.log(`Soy superhéroe ${this.name}`)
  }
}
function Hero(name) {
  const hero = Object.create(heroMethods)
  hero.name = name
  return hero
}
const zelda = Hero('Zelda')
const link = Hero('Link')
```

En éste ejemplo, no estamos haciendo solamente una *copia* de los métodos que existen en `heroMethods`, sino que estamos haciéndole la  **herencia prototipal** ¿qué quiere decir esto? quiere decir que no le estamos agregando funciones a las instancias, sino que se las estamos agregando a su objeto `__proto__` y aunque en la consola no sea visible a primera vista, sí le pertenecerá a cada instancia. Una forma más reducida aún para hacer esto sería la siguiente:
```javascript
function Hero(name) {
  const hero = Object.create(Hero.prototype)
  hero.name = name
  return hero
}
Hero.prototype.saludar = function() {
  console.log()
}
const zelda = Hero('Zelda')
const link = Hero('Link')
```

De esta forma entendemos cómo es que JavaScript genera instancias de los objetos, es decir, hace la *herencia prototipal*, con lo cúal será fácil entender lo que hace la palabra reservada `new`, que realmente es instancias un nuevo objeto, dada una función *consttuctora*, asociando todo a un contexto propio del prototipo que guarda en una variable llamada obligadamente `this`, con lo cuál ya no tiene que haber un return, pues va implícito con el uso de una función constructura y el uso de `new`:
```javascript
function Hero(name) {
  //const hero = Object.create(Hero.prototype)
  this.name = name // se usa this
  // return hero
}
Hero.prototype.saludar = function() {
  console.log(`New ${this.name}`)
}
const zelda = new Hero('Zelda')
const link = new Hero('Link')
```

La herencia prototipal es una forma muy completa para generar herencia en JavaScript, puer nos permitirá crear instancias con todos los métodos que tenga el prototipo a partir del cuál fue creado, al final, todo en JS es un objeto y podremos acceder a todos los métodos de `Object`. Inspeccionemos el prototype de alguna de nuestras instancias:
```javascript
const prototypeOfZelda = Object.getPrototypeOf(zelda)
console.log(prototypeOfZelda === Hero.prototype) // true
```

Si nosotros le agregamos algún método o propiedad a un prototype, éste existirá automáticamente en todas sus instancias (pero en el prototype):
```javascript
Hero.prototype.fight = function() {
  console.log('Fight!')
}
const protoypeOfPrototypeOfZelda = Object.getPrototypeOf(Hero.prototype)
protoypeOfPrototypeOfZelda.hasOwnProperty('fight') // true
```

La búsqueda de funciones o properties dentro de un prototype se escala desde el elemento que estamos usando hasta el objeto `Object`, que es el punto mas alto en esta cadena de herencia prototipal.

# Parsers y Abstract Syntax Tree
La Web no siempre ha sido como ahora, JavaScript fue ntroducido por primera vez con Netscape. JS en sí mismo servía para hacer tareas muy simples, se interepretaba línea por línea, hasta la fecha esto sigue siendo así pero de una manera más moderna que introdujo Google rediseñando el motor de JS. Lo que hace el navegador es aproximadamente lo siguiente:
* Recibe código fuente
* Parsea el código y produce el Abstract Syntax Tree (AST)
* Se compila a bytecode y se ejecuta
* Se optimiza a machinecode y se reemplaza el código base

![funcionamiento javascript](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/how-js-works.jpg?raw=true)

Analicemos primeramente la parte del **parser**

## Parser
Toma nuestro código fuente y lo lee, pues la computadora tiene que descomponerlo para procesarlo, lo que hace primeramente es descomponerlo en **tokens**, ej: la declaración de variables y funciones, si algo no hace sentido es cuando ocurre un *SyntaxError*. Este proceso ocupa una 15-20% del tiempo de procesamiento, por lo cuál se dice que la mayoría del código de JS nunca se ejecuta. De ello la importancia de hacer **bundling** y **code splitting** o prácticas como las **Single Page Applications**.

Hay dos modos de hacer parsing en **V8** (usado por Chrome y Node):
|Eager Parsing:|Lazy parsing:|
|-|-|
|Encuentra errores de sináxis|Doble de velocidad que eager|
|Crea el AST|No crear el AST|
|Construye scopes|Construye los scopes parcialmente|

En este [enlace](https://esprima.org/) podemos ver ejemplos de los tokens generados por el parser.

## AST
Es un grafo (estructura de datos) que representa un programa. Se usa en:
* JavaScript
* Bundlers: Webpack, Rollup, Parcel
* Transpilers: Babel
* Linters: ESLint, Prettify
* Type Checkers: TypeScript, Flow
* Syntax Highlighters

En este [enlace](https://astexplorer.net/) podemos ver gráficamente el AST.

Un ejemplo del uso de AST para verificar el lineto de una línea de código es el de la siguiente imagen, donde verificamos que una declaración de variable sea de tipo `const`, que almacene un número y que su nombre esté en mayúsuclas, todo esto lo logramos validando nodo por nodo de AST en esta página que nos permite inspeccionarlo, luego podríamos llevar esta función hacia un linter local e incluso agregar funciones que sean *fixers* de nuestro código:

![ejemplo de AST](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/ast-example-eslint.jpeg?raw=true)

## Profiling Data
Cuando el intérprete genera el bytecode, el *Optimizer compiler* puede llevar a cabo la optimización de código que sea muy reutilizado, si éste no se utiliza, se deoptimizará, por ello es importante mandar llamar las funciones con los mismo tipos de input.

## Otros motores de JS
Existen otros motores de JavaScript que siguen una estrucutra parecida, agregan más capas de optimizacioón como:
* **SpiderMonkey** (Firefox) - 2 capas de optimización
* **Chakra** (Edge) - 2 capas de optimización
* **JavaScriptCore** (Safari) - 3 capas de optimización

# Funcionamiento de JavaScript en un solo hilo
JavaScript corre sobre un solo hilo, para esto utiliza el *event loop*. Consideremos que JS se organiza usando dos tipos de estructuras de datos:
* **Stack**: lleva rastro de dónde está el programa en cada momento (call stack), es una estructura **LIFO**:
  * Comienza vacío y se le puede hacer push de múltiples elementos (pero para sacar hay que hacer pop)
  * En el stack está también almacenada la información sobre el scope de las funciones. Illustración del callstack:
    ![call stack](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/callstack-animation.gif?raw=true)
  * En el caso de las funciones asíncronas, éstas se *mandarán* a procesar, pero solo se ejcutarán en nuestro programa una vez que se haya vaciado el callstack. De aquí surge el siguiente con concepto que es el *queue*:
* **Task queue**: es una estrucutra de datos tipo **FIFO**, a la que obedecen las tareas asíncronas una vez que pueden ser ejecutadas, dicho de otro modo, es una cola de tareas que está esperando entrar al callstack.
* **Event loop**: es el ente encargado de verificar constantemente el estado del stack para saber si puede cargarle la cola de tareas resueltas y éstas puedan ejecutarse. Esta es una illustración:
  ![call stack](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/eventloop-animation.gif?raw=true)
  > tomemos en cuenta que las promesas se forman en otra cola de tareas dedicada a las promesas.
* **Memory heap**: almacena información sobre las variables de manera aleatoria

# Getters y Setters
Son funciones que se emplean dentro de los objetos que nos permiten tener propiedades virtuales para computar otras propiedades. Consideremos el siguiente ejemplo, en el que computaremos el resultado de elevar una base a un exponente pero sin guardar dicha base como propiedad de nuestra clase:
```javascript
class ExponentCalculator {
  constructor(exp) {
    this.exponent = exp
  }
  set base(value) {
    this.result = Math.pow(value, this.exponent)
  }
  get result() {
    return `The result of this instance is: ${this.result}`
  }
}
let squaredCalculator = new ExponentCalculator(2)
squaredCalculator.base = 2
console.log(squaredCalculator.result) // 4
```

Nótese que `base` no es una propiedad per se de nuestra clase, sino que solamente es un `setter` que nos sirve para recibir un valor y hacerle algún *tratamiento*, luego el `getter` nos ayudará para poder *sacar* información almacenada en nuestra clase, misma que también puede estar modificada a la salida.

# Proxy
Nos permitirán interceptar y definir un comportamiento customizado sobre operaciones que se hagan con un objeto. Básicamente es el mismo concepto de getters y setters pero con muchas más opciones ya que no operan sobre una propiedad sino sobre un objeto, ver la [referencia](https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/Proxy).

Creemos un ejemplo donde interceptamos llamadas para leer una propieda, si la propiedad existe, la regresamos, si no existe, entonces sugerimos una que pueda funcionar:
```html
<html>
  <head>
    <title>Proxy</title>
  </head>
  <body>
    <script src="https://unpkg.com/fast-levenshtein@2.0.6/levenshtein.js"></script>
    <script>
      const target = {
        red: 'Rojo',
        green: 'Verde',
        blue: 'Azul',
      };
      const handler = {
        get(obj, prop) {
          if(prop in obj) {
            return obj[prop];
          }
          const suggestion = Object.keys(obj).find(key => {
            return Levenshtein.get(key, prop) <= 3;
          });
          if(suggestion) {
            console.log(
              `${prop} no se encontró. Quisiste decir ${suggestion}?`
            );
          }
          return obj[prop];
        },
      };
      const p = new Proxy(target, handler);
    </script>
  </body>
</html>
```
> Nótese que en este ejemplo, solo se está utilizando una *trampa* propia de los Proxy, hay muchas más opciones para implementar este feature de JavaScript.

# Generators
Los generators son funciones especiales que podemos iniciar, detener en un punto, irnos a otra función y eventualmente regresar a su ejecución donde la dejamos y la función recordará su contexto. Veamos un ejemplo:
```javascript
function* simpleGenerator() {
  console.log('Generator start')
  console.log('Generator end')
}
const gen = simpleGenerator()
gen.next()
// start
// end
// {value: undefined, done: true}
```
> Nota. La función generator se crea usando un asterisco (*) después de la palabra function, pero esta no es una función ejecutable, al emplearle el método next veremos a la salida los valores que se muestran en los comentarios.

¿Qué significa el objeto que vemos a la salida?, `value` puede ser un valor que se está retornando en ese momento por parte del generator y `done` es una bandera que nos indica si ya terminó la ejecución, en este caso, en un solo llamado a `next` terminamos la ejecución pero nunca interrumpimos para retornar nada. Veamos cómo retornar valores:
```javascript
function* simpleGenerator() {
  console.log('Generator start')
  yield 1
  yield 2
  console.log('Generator end')
}
const gen = simpleGenerator()
gen.next()
// start
// {value: 1, done: false}
gen.next()
// start
// {value: 2, done: false}
gen.next()

// end
// {value: undefined, done: true}
```
En este ejemplo, vemos cómo cada llamada al método next, significa un paso más en la ejecución de la función, misma que puede retornar tantos valores por medio de la palabra reservada `yield` como queramos.

## Generators infinitos
Podemos construir un generator que esté devolviendo valores de manera indeterminada. Veamos el siguiente ejemplo:
```javascript
function* idMaker() {
  let id = 1
  while(true) {
    yield id
    id = id + 1
  }
}
```

Esta función, no tiene un final determinado, pues siempre retornará un valor incrementado a modo de id. Podríamos resetear el valor incrementado del id de la siguiente manera: 
```javascript
function* idMaker() {
  let id = 1
  let reset
  while(true) {
    reset = yield id
    if(reset) {
      id = 1
    } else {
      id = id + 1
    }
  }
}
idMaker.next(true)
// {value: 1, done: false}
```

Ésta sería un uso más complejo de generators infinitos: la creación de determinado número de elementos de la serie fibonacci:
```javascript
function* fibonacci() {
  let a = 1;
  let b = 1;
  while(true) {
    const nextNumber = a + b
    a = b
    b = nextNumber
    yield nextNumber
  }
}
```
> cada ejecución nos dará un nuevo elemento de la serie

# APIs del Navegador

## Fetch
Las peticionas AJAX, fueron todo un hito en la web, puer nos permitieron cargar infromación sin tener que recargar toda la página. Para esto, se utiliza el método `XMLHttpRequest`, pero su implementación llega a ser un poco compleja en ciertos casos. Es por esto que llega la función `fetch` para simplificar nuestros requests, con la ventaja de que aprovecha las promesas. La única desventaja era que no había modo de cancelar una petición, lo cuál es muy importante en Single Page Applications, en donde puede que una respuesta asíncrona rompa flujos o sobrecargue datos innecesarios. Para ello llegó `abortController`. Consideremos este ejemplo, en el que queremos cargar una imagen muy pesada.

Consideremos las siguientes funciones asíncronas, que están preparadas para hacer el request de una imagen y asignarla a un elemento img de nuestro HTML, con la opción de abortar dicha petición:
```javascript
startButton.onclick = async function() {
  startLoading()
  // intanciamos AbortController
  controller = new AbortController()
    try {
      // pasamos la señal del controller
    const response = await fetch(url, {signal: controller.signal})
    // extraemos el archivo binario de la respuesta
    const blob = await response.blob()
    // generamos una url para colocar al elemento img
    const imgUrl = URL.createObjectURL(blob)
    img.src = imgUrl
  } catch(error) {
    console.error(error.message)
  }
  stopLoading()
}
stopButton.onclick = async function() {
  // abortamos el fetch
  controller.abort()
  stopLoading()
}
```

## IntersectionObserver
Esa funcionalidad, nos permite observar un elemento en nuestro navegador, para definir un mínimo umbral del mismo que tiene que estar visible para luego llecar a cabo otras acciones. Esta funcionalidad, la podemos utilizar, por ejemplo, para detener la reproducción de un video cuando el usuario hace scroll de manera que éste ya no es visible en la ventana.

Consideremos la siguiente clase, encargada de implementar ÌntersectionObserver`, que recibe un reproductor de un video (media) y tiene las funciones de `play` y `pause` para pausarlo o reproducirlo según el scroll del usuario:
```javascript
class AutoPause {
  constructor() {
    this.threshold = 0.25
    this.handleIntersection = this.handleIntersection.bind(this)
  }
  run(player) {
    this.player = player
    // el handler recibe la información de la salida/entrada del umbral
    const observer = new IntersectionObserver(this.handleIntersection, {
      threshold: this.threshold
    })
    observer.observe(this.player.media)
  }
  // lógica del handler
  handleIntersection(entries) {
    const entry = entries[0]
    const isVisible = entry.intersectionRatio >= this.threshold
    isVisible ? this.player.play() : this.player.pause()
  }
}
export default AutoPause
```

## VisibilityChange
Es otra API del navegador, que se coloca como listener en una page para que escuche cuando cambiamos de tab y ejecute luego una acción.

```javascript
document.addEventListener('visibilitychange', () => {
  console.log(document.visivilityState) // hidden / visible
})
```

## Service Workers
Una tendencia en el desarrollo, son las PWAs, una de las ventajas que ofrecen, es la capacidad de funcionar offline. Para esto se utilizan los **Service Workers**. Que son una capa enmedio del navegador y del internet, tienen similitud con los proxies, ya que interceptan peticiones: las toman, buscan la respuesta y antes de devolverla al browser la guardan en caché, entonces la proxy en lugar de ir al servidor, podrá tomar los datos de caché si por algún motivo el usuario perdió la conexión y le permitirá seguir usando la aplicación.

Para agregar un serviceworker, primero tenemos que identificar si el navegador en cuestión tiene ServiceWorkers:
```javascript
// index.js
if('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/sw.js').catch(err => {
    console.error(err.message)
  })
}
```

> Nótese que estamos añadiendo un archivo per se, llamado `sw.js`, que es donde estará la lógica de nuestro ServiceWorker, por otro lado, también tenemos un catch por si el navegador no cuenta con la feature.

En el nivel más alto de nuestro proyecto, agregaremos el archvio `sw.js`. Para empezar con nuestro código, es importante considerar que los SWs se tienen que *instalar* en nuestro navegador, veamos su implementación:
```javascript
// sw.js
const VERSION = 'v1'
/**
 * self hace referencia al propio sw
 * El evento install, se mandará llamar cuando se halla instalado correctamente el sw
 */
self.addEventListener('install', event => {
  // creamos un precaché, que es una lista de cosas que queremos nuestro sw mantenga en cache, indicamos que espere hasta que el cache se complete
  event.waitUntil(precache()) // -> si se resuelve con éxito, ya podemos ver nuestro SW instalado exitosamente en el browser con los recursos que le indicamos
})
async function precache() {
  // abrimos un cache puntual (promesa)
  const cache = await caches.open(VERSION)
  // una vez que hayamos abierto la instancia de la cache, agregamos todos los recursos que queremos agregarle (promesa que esperará waitUntil)
  return cache.addAll([
    '/',
    '/index.html',
    '/assets/index.js',
    '/assets/MediaPlayer.js',
    '/assets/plugins/AutoPlay.js',
    '/assets/plugins/AutoPause.js',
    '/assets/index.css',
    'assets/BigBuckBunny.mp4'
  ])
}
// El evento fetch, ocurrirá cada que hagamos un request al servidor
self.addEventListener('fetch', event => {
  // extraemos las petición
  const request = event.request
  // filtramos solamente las peticiones de tipo GET
  if(request.method !== 'GET') {
    return
  }
  // buscamos los datos de esta petición en caché para ver si no es necesario pedirlos al servidor
  event.respondWith(chacheReponse(request)) // -> si esto funciona, podremos trabajar con archivos que estén cacheados previamente aunque no tengamos conexión
  /** Estrategia catch & network: nos permitirá actualizar los archivos
   * para que el usuario no se quede por siempre con lo que tiene en caché
   * y vaya a perderse de actualizaciones
   */
  event.waitUntil(updateCache(request))
})
async function chacheReponse(request) {
  // instanciamos cache
  const cache = await caches.open(VERSION)
  // buscamos si hay una copia del request en cache
  const response = await cache.match(request)
  // contestamos con lo que tengamos en cache o en su defecto, hacemos el fetch normalmente
  return response || fetch(request)
}
async function updateCache(request) {
  // instanciamos cache
  const cache = await caches.open(VERSION)
  // hacemos la petición al servidor
  const response = await fetch(request)
  // escribimos en cache las actualizaciones
  return cache.put(request, response)
}
```
