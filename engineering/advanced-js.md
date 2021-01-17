# Some concepts

Para inicializar un poryecto con todas las banderas en `y` usamos `npm init -y``
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

Hay dos modos de hacer parsing en V8 (usado por Chrome y Node):
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

