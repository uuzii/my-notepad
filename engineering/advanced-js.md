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