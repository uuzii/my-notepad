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


