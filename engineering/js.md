🚀 v0.2.0

# Funciones declarativas y expresivas
* Las funciones declarativas, son aquellas que se declaran con la palabra reservada `function`
* Las funciones expresivas, son aquellas funciones anónimas que se guardan en una variable.

La diferencia entre ambas, es que a las funciones declarativas se les aplica hoisting, y a la expresión de función, no. Ya que el hoisting solo se aplica en las palabras reservadas var y function.

Lo que quiere decir que con las funciones declarativas, podemos mandar llamar la función antes de que ésta sea declarada, y con la expresión de función, no, tendríamos que declararla primero, y después mandarla llamar.

# Arrow functions {#arrow-functions}
Son una manera alterna para generar funciones. Previamente tenemos que considerar que en JavaScript se pueden almacenar funciones *anónimas* dentro de una variable:

```javascript
const esMayorDeEdad = function(edad) {
  return edad >= 18
}
// esMayorDeEdad(23) = true
```

Lo anterior se puede escribir de la siguiente manera:
```javascript
const esMayorDeEdad = edad => edad >= 18
// esMayorDeEdad(23) = true
```

Aunque la funcionalidad es la misma, ambas funciones no son exatamente iguales, de primera instancia vemos las siguientes diferencias:
* Podemos obviar los paréntesis si solo se le pasa un solo parámetro
* Podemos eliminar las llaves y la palabra reservada `return` si la función solo retorna un valor

## this en una arrow function
En las funciones regulares, `this`siempre representará el objeto en el que han sido llamadas, podría represntar el `window`, `document`o un `button`, por ejemplo.
> Con una arrow function, `this`, siempre representatá el objeto que define la arrow function (por lo general, `window`)
Fuente: [w3](https://www.w3schools.com/js/js_arrow_function.asp)

# Scope
Tradicionalmente se dice que es el alcance que tienen las variables, significa que, dependiendo dónde declaramos una función o variable, se definirá en qué lugar se pueden utilizar.

Los tipos de scope son:
* Scope Global. También conocido como `window`, es el ámbito global de todo nuestro código de Javascript
* Scoper Local. Se delimita por un *bloque*, que bien puede ser el contenido de una función, realmente los bloques se delimitan por las llaves `{}`
Veamos el siguiente fragmento:
```javascript
var name = "Uzi"
function printName() {
  var lastName = "Rodriguez"
  console.log(name + " " + lastName);
}
```

En este ejemplo, es posible acceder desde un scope local (delimitado por la función, a un scope global, mas no es posible acceder desde el scope global al local). En general, no es buena práctica modificar una variable global dentro de un contexto local, en su lugar, las variables locales deberían ser las únicas que se usen dentro de su bloque, pero si exisitiera la necesidad de modificarlas, podemos accederlas usando `window.varGlobal`;

# Hoisting
Es cuando las variables y las funciones se declaran antes que se procese cualquier otra parte del código. Esto sucede en versiones iguales o menor a *ECMAScript 5* ya que solo aplica a las palabras reservadas `var` y `function`; a partir de ECMAScript 6 se introdujo `let`y `const, que son formas de declarar variables que nos evitarán esto.
```javascript
console.log(name);
var name = 'Uzi';
```

En el programa anterior, veremos un `undefined`, dado que el navegador no identificó esta varibale, aquí sucede el fenómeno *hoisting*, que se debe a que, en el *JIT compiler* al llegar al console log, se prepara un espacio en memoria para la variable pero no tiene un valor que asignarle.

En el caso de las funciones, podemos ejecutarlas antes que éstas se declaren, puesto que son lo primero que se identifica en el *JIT compiler*, incluso antes que las variables.

# Coerción
Muchas veces vemos en JavaScript suceden fenómenos como los siguientes:
```javascript
4 + '7' // '47'
4 * '7' // 28`
```

Aquí sucede que, el lenguaje trata de ayudarnos a castear los datos de acuerdo a la operación que estamos tratando de hacer.

Hay dos tipos de coerción:
* Implícita. El lenguaje nos ayuda y cambiar de un tipo de valor a otro tipo de valor
* Explícita. Es cuando nosotros obligamos a que un tipo de valor se convierta a otro. Para hacer esto, podemos usar los métodos de datos primitivos, por ejemplo:
```javascript
String(20)
```

Convertirá la varibale de tipo númber a un string

# Valores truthy y falsy
Los usaremos para identificar ciertos valores como verdaderos o falsos para estructuras de control como el `if`. Los siguientes valores se identifican como *falsy*:
* `false`
* `''`
* `0`
* `NaN`
* `undefined`

Los siguientes valores o criterios son *truthy*;
* `true`
* Cualquier número exepto `0`
* Cualquier string con al menos un caracter `' '`
* `[]`
* `{}`
* `function() {}`

# Prototype Object
JavaScript es un lenguaje basado en prototipos, no tanto en clases, como otros lenguajes. Si bien, hoy en día existe la palabra reservada `class`, esto no es sino *sugar syntax*, ya que en Js, todos los elementos son prototipos. Uno de los más importantes, es el prototipo `Object`, por ello es importante identificarlo y saber manipularlo. La anatomía de un objeto es la siguiente:
```javascript
var obj = {
  name: 'Name',
  lastName: 'Last Name',
  age: 23,
  male: true,
  greeting: function() => { console.log(`Hola ${this.name}`) }
}
```

Como vemos, un objeto es una estrucutra de *llave-valor*, a la que podemos acceder de la forma: `obj.key`. Vemos que nos permite almacenar todo tipo de datos y funciones. En las versiones más modernas de Js, podemos mandar llamar solo alguna propiedad de un objeto de la siguiente manera:
```javascript
var obj = {
  name: 'Name',
  lastName: 'Last Name'
}
function printName({ name }) { console.log(name.toUpperCase) }
printName(obj)
```

## Destructurar un objeto
Destructurar un objeto consiste en generar variables con los mismos nombres de las propiedades de un objeto:
```javascript
var obj = {
  name: 'Name',
  lastName: 'Last Name'
}
var name = { obj }
// name = 'Name'
```

## Pasar parámetros por referencia
Al pasar objetos en JavaScript como parámetro a una función, realmente le estamos pasando una referencia del mismo
```javascript
var obj = {
  name: 'Name',
  age: 23
}
function cumpleanios(person) {
  person.age += 1
}
cumpleanios(obj)
// obj.age = 24
```

Vemos que se modifica el valor del objeto, si queremos que se gener un nuevo objeto, simulando que pasamos el valor del objeto solamente, tenemos que usar el *spread operator (...)*, que no es otra cosa, sino generar una copia de un objeto dentro de otro:
```javascript
var obj = {
  name: 'Name',
  age: 23
}
function cumpleanios(person) {
  return {
    ...person,
    age: person.age += 1
  }
}
newObj = cumpleanios(obj)
// newObj = { name: 'Name', age: 24 }
```

Entendiendo esto, sabremos que no podemos hacer una comparación directa entre objetos, es decir: `{}` nunca será igual a otro `{}` aunque tengan el mismo contenido, pues en memoria tendrán diferentes ubicaciones, a menos que haya sido creado apuntando al mismo objeto.

para declarar un objeto, podemos emplear diferentes técnicas:
* Usar la forma declarativa `var obj = {}`;
* Usando la palabra reservada `new`: `var person = new Object()`
* Usando la función `Object.create()`
* Definiendo un constructor y posteriormente instanciar nuevos objetos a partir del mismo:
```javascript
function Person(name, lastName) = {
  this.name = name;
  this.lastName = lastName
}
var me = new Person('Uzi', 'Rodriguez');
```

  Una vez definido un prototipo, solo es posible agregarle propiedades y métodos mediante la sintaxis (no usar arrow functions):
```javascript
Obj.prototype.newMethod = function() { ... }
```

# Clases {#classes}
Las clases en Js, no son iguales que en otros lenguajes, realmente son prototipos con *sugar syntax*. Si queremos, por ejemplo crear un prototipo (o función constructura) e instanciarlo, hacemos lo siguiente:
```javascript
function Persona(name) {
  this.name = name
}
var person = new Persona('Uzi')
```

Con esto, nos damos cuenta de que no existe la herencia de clases, sino solo la *herencia prototipal*. Los prototipos únicamente buscan si pueden responder a los métodos o properties que señalemos, si no puede, hace un escalamiento en su prototipo padre y así sucesivamente hasta llegar al prototype Object.

```javascript
function heredaDe(sonPrototype, fatherPrototype) {
  var fn = function() {}
  fn.prototype = fatherPrototype.prototype
  sonPrototype.prototype = new fn
  sonPrototype.prototype.constructor = sonPrototype
}

function Persona(name) {
  this.name = name
}

Persona.prototype.saludar() = function() { console.log('Hola ', this.name) }

function Desarrollador(name, lastName) {
  this.name = name;
  this.lastName = lastName;
}

heredaDe(Desarrollador, Persona)
```

De la manera mencionada, se hace una herencia, vemos que realmente la herencia consiste en añadir el prototipo a u  hijo mediante una función vacía así como el constructor, lo que sucederá, es que el prototipo hijo obtendrá las propiedades y métodos de su padre. Si se le pide que resuelva alguna propiedad o método que no tiene en su prototype y no las tiene, las buscará en los prototipos padres (__proto__) hasta llegar a Object, que es el prototipo más grande.

A partir del 2015, JavaScript se ha actualizado año con año a través del estándar *ECMA*, en estas actualziaciones, se ha agregado cierta sintáxis que nos permite generar herencias de una manera más simple (aunque en el fondo siguen siendo prototipos) lo haremos de esta manera:

```javascript
class Persona {
  constructor(name, lastName) {
    this.name = name
    this.lastName = lastName
  }
  greeting() {
    console.log('Hola, me llamo', this.name + ' ' + this.lastName)
  }
}
```

Para generar herencia en esta nueva sintáxis, usaremos la palabra reservada *extends*:

```javascript
class Persona { ... }
class Desarrollador extends Persona {
  constructor(name, lastName) {
    super(name, lastNAme); // para llamar al constructor de la clase padre
  }
  greeting() {
    console.log('Hola, me llamo', this.name + ' ' + this.lastName + ' y soy developer')
  }
}
```


# Prototype Array
Loa arrays son variables que alamacenan un conjunto de datos

## Algunos de sus métodos son:
* `Array.length`: muestra el número de elementos que tiene
* `Àrray[pos]`: muestra el elemento en la posición indicada
* `Array.push(element)`: agrega un elemento al final del array
* `Array.pop()`: eliminar el último elemento al final del array
* `Array.unshift(element)`: agrega un elemento al inicio del array
* `Array.shift()`: eliminar el primer elemento del array
* `Array.indexOf(element)`: muestra el index de un elemento en el array

## Métodos para recorrido de array
Existen ciertos métodos que nos ayudan a analizar nuestros arrays y generar nuevos arrays con los resultados indicados:

### filter()
Nos genera un **nuevo** array que contenga los elementos que cumplan el criterio que indiquemos en una función que recibe como parámetro, ejemplo:
```javascript
var articulos = [
  { name: 'Laptop', prize: 10000 },
  { name: 'Libro', prize: 350 },
  { name: 'Audífonos', prize: 550 },
  { name: 'Cable', prize: 350 }
];
var elementosFiltrados = articulos.filter(function(articulo){
  return articulo.prize > 600;
});
// elementosFiltrados = [{ name: 'Laptop', prize: 10000 }]
```

### map()
Nos genera un **nuevo** arreglo, en el que podemos generar una nueva estructura de datos, *mapeándolos* en cada ítem del arreglo original:
```javascript
// ...
var nombres = articulos.map(function(articulo){
  return articulo.name;
});
// elementosFiltrados = ['Laptop', 'Libro', 'Audífonos', 'Cable']
```

### find()
Nos ayuda a encontrar algo dentro de un arreglo, este método no modifica el array original, sino que genera un nuevo array en caso de que el query buscado aparezca
```javascript
// ...
var encuentraArtículo = articulos.find(function(articulo){
  return articulo.name === 'Cable';
});
// encuentraArtículo = { name: 'Cable', prize: 350 }
```

### some()
Genera un boolean si encuentra en los ítems del array el query indicado en la función que se le pasa como parámetro
```javascript
// ...
var articulosBaratos = articulos.some(function(articulo){
  return articulo.prize < 400;
});
// articulosBaratos = true
```

### reduce()
Reduce un array a un valor mínimo, en el siguiente ejemplo, mediante el uso de un for, generamos un acumulador de la cantidad de libros que tienen todas las personas en una lists:
```javascript
var personas = [
  { name: 'Person 1', books: 3 },
  { name: 'Person 1', books: 1 },
  { name: 'Person 1', books: 5 },
  { name: 'Person 1', books: 10 }
]
var acum = 0
for(var i = 0; i < personas.length; i++) {
  acum = acum + personas[i].books
}
// acum = 19
```
Pero con `reduce`, podemos hacer esta acción de una manera alternativa:
```javascript
// ...
const reducer = (acum, persona) => {
  return acum + persona.books
}
var total = personas.reduce(reducer, 0);
// acum = 19
```
Donde el primer argumento, es la función reductora, el segundo argumento es el valor inicial de un acumulador

# Asincronismo

Para empezar a hablar de asincronismo, es importante entender, que en Js es posible pasar funciones como parámetros de una función, recordemos que las funciones son consideradas *first class citizens*:

```javascript
function greeting(fn) {
  console.log('Hola')
  if(fn) {
    fn('uzi', 'rodriguez', true);
  }
}
function replyGreeting(name, lastName, isDev) {
  console.log('Buen dia ' + name + ' ' + lastName)
  if(isDev) {
    console.log('No sabia que eres dev')
  }
}
greeting(replyGreeting)
```

## ¿Cómo funciona el asincronismo?

Tomemos en cuenta que, JavaScript no puede ejcutar múltiples tareas a la vez, pero aún así, tiene la capacidad de delegar la ejecución de ciertas funciones a otros procesos. Este modelo de concurrencia, se llama *Eventloop*.

Para entenderlo, consideremos que la pila de ejecución de funciones que obedecerá JS, se llama *callstack*. En esta pila, existen determinados procesos, que se delegarán directamente al navegador, por ejemplo una petición a un servidor. Dichas funciones llevan consigo otra función denominada *callback*, misma que, una vez resuelta la tarea por el navegador, estará lista para ser ejecutada.

Mientras tanto, se seguirá ejecutando el programa principal, pero no es hasta que el programa principal se quede sin funciones en su *callstack*, que irá a ver qué *callbacks* ya están listos en la *pila de ejecuciones* que se llena con todos los callbacks "resueltos" por el navegador. Por ello es importante no saturar el callstack con funciones muy pesadas, ya que los callbacks se quedarán bloqueados hasta que el programa principal se libere.

Nótese que las funciones que se "delegarán" al navegador, son las que corresponden a API's del mismo, funciones como `setTimeout`, `alert`, `fetch`, entre otras.

Veamos un ejemplo, en el siguiente código, evidentemente veremos en consola cada letra seguida de la otra en el mismo orden del código, pues la función `console.log` solo se carga en el callstack:

```javascript
console.log('a')
console.log('b')
console.log('c')
```

Pero si introducimos un proceso asíncrono, no se cumplirá dicho orden, dado que la segunda línea, se entregará al navegador para que ejecute una función después de 0 segundos, si bien, esto no representa ningún tiempo, veremos que su callback se ejecutará al final, puesto que JS siguió con el programa principal y hasta que se liberó de sus tareas, fue por el callback que estaba en lista de espera:

```javascript
console.log('a')
setTimeout(() => console.log('b'), 0)
console.log('c')
// a c b
```

Si delegáramos un proceso al navegador que fuera muy ligero, pero nuestro programa principal fuera demasiado pesado, veremos que hasta después de que termine su callstack, se ejcutará nuestro callback:

```javascript
setTimeout(() => console.log('d'), 1000)
for(var i = 0; i< 100000000000; i++) {}
```

Aquí radica la importancia de nosaturar nunca el eventloop con tareas muy pesadas.

## Manejando el asincronismo

Dado que las respuestas de las operaciones asíncronas no necesariamente se ejecutarán en un orden determinado, surge la necesidad de encolar una función después de otra, esto lo podemos hacer de la siguiente manera:

```javascript
function getContentFromServer(param, fn) {
  fetch(`http://example.com/${param}`)
    .then(data => {
      console.log(data)
      if(fn) {
        fn()
      }
    })
    .catch(error => console.error('Error: ', error))
}
getContentFromServer('1', function() {
  getContentFromServer('2', function() {
    getContentFromServer('3')
  })
})
```

Donde observamos que la función asíncrona recibe como parámetro una función **anónima**, misma que seria la siguiente en orden de ejecución. Pero escribir nuestro código de esta manera, haría que nuestro código se hiciera horizontal, a este fenómeno se le llama *callback hell*.

Tambié observamos que hemos pasado una función en un `catch()`, mismo que nos ayudaría si hubiese algún problema en el navegador para ejecutar la llamada a servidor.

## Promises

Para evitar que se genere un *calback hell*, existen el objeto Promise, las promesas son valores que aún no conocemos, es decir, lugares en los que habrá un valor cuando el navegador resuelva una operación asíncrona. Las promesas tienen tres posibles estados:
* **fullfilled**. Se obtendrá cuando se resuelva el valor de la promesa, posterior a ello ejecutaremos el método `.then(val =>)`
* **rejected**. Se obtendrá cuando el navegador no pueda resolver el valor de la promesa y posteriormente se ejecutará el método `.catch(err =>)`

Así luce una promesa:

```javascript
new Promise(function(resolve, reject) {
  if(success) {
    resolve(data)
  } else {
    reject(err)
  }
})
.then(val => {
  // actions with val
  return anotherPromiseChained()
})
.catch(err => {})
```

Donde:
* `resolve` es una función que se ejecutará cuando el resultado sea exitoso
* `reject` es una función que se ejecutará cuando el resultado sea fallido
* `then` podrá contener acciones sucesivas cuando el resultado sea exitoso y podrá retornar otros objetos Promise para encadenar procesos
* `catch` podrá contener acciones sucesivas cuando el resultado sea fallido, no importa cuántos then sucedan a la promesa

## Generar múltiples promesas en paralelo

Veamos una forma en la que podemos generar un `Array` de promesas:

```javascript
var ids = [1,2,3,4,5,6,7]
var promesas = ids.map(id => new Promise((resolve, reject) => { /*resolve and reject logic*/ })
})
// ids -> array de promesas
Promise
  .all(promesas)
  .then(respuestas => {
    // array de respuestas
   })
   .catch(onError)
```

Donde ejecutaremos múltiples promesas en paralelo a través de `Promise.all()` y obtener un array de sus respuestas (también cachar el error en cualquier momento)

## Async-await

Es la forma más moderna de realizar tareas asíncronas:

```javascript
async function getTooManyResponses() {
  var ids = [1,2,3,4,5,6,7]
  var promesas = ids.map(id => new Promise((resolve, reject) => { /*resolve and reject logic*/ })
  })

  try {
    var respuestas = await Promise.all(promesas)
    console.log(respuestas)
  } catch(id) {
    onError(id)
  }
}

getTooManyResponses()
```

Donde la palabra reservada `await` impide que se ejecuten las líneas de código contiguas hasta que se resuelvan los procesos asíncronos ordenados a su derecha, todo esto lo englobamos en un bloque `try-catch` que nos permitirá saber si hay algún fallo y a su vez toda la función debe ser declarada como `async`; el llamado a esta será normal.

# Memoización

La memoización nos permite ahorrar procesamiento, guardando algunos datos temporalmente. En el siguiente ejemplo, se calcula el factorial de un número mediante una función recursiva, posteriormente, usamos memoización para guardar factoriales menores y ahorrarnos el cómputo de los mismos al calcular facotriales más grandes guardándolos en un objeto llamado `cache`:

```javascript
function factorial(n) {
  if(!this.cache) {
    this.cache = {}
  }
  if(this.cache[n]) {
    return this.cache[n]
  }
  if(n === 1) {
    return 1
  }
  this.cache[n] = n * factorial(n - 1)
  return this.cache[n]
}
```

# Closures

Un closure es una función que *recuerda el estado de las cosas cuando fue invocada*, Dicho de un modo más formal, es una función interna, que tiene acceso al scope de su función externa incluso después de que esta haga un return. El siguiente ejemplo, utiliza closures para generar saludos de diversos países:

```javascript
// External function
function createGreeting(phrase) {
  // internal function
  return function(name) {
    console.log(`Hola ${name} ${phrase}`)
  }
}
const mexicanGreeting = createGreeting('wey')
const colombianGreeting = createGreeting('parse')
mexicanGreeting('Uzi')
colombianGreeting('Juan Valdez')
```

Donde `createGreeting`es como una función generadora de funciones que persisitirá sus variables, cuando se invoque la función generada, veremos cómo es que se persisten dichos valores.

Los closures nos permiten emular las variables *privadas* que por default no existen en JavaScript:

```javascript
function makeCounter(n) {
  let count = n

  return {
    increase: function() {
      count++
    },
    getCount: function() {
      return count
    }
  }
}
let counter = makeCounter(7)
console.log(counter.count) // No existe en el scope global
console.log(`The count is: ${counter.getCount()}`) // 7
```

# Cambiar de contexto al llamar una función

Veamos el siguient ejemplo, en el que al querer acceder a un contexto que no es el propio, ocurre un error

```javascript
const uzi = {
  name: 'uzi',
  lastName: 'rodriguez',
}
function greeting() {
  console.log(`Hi, my name is ${this.name}`) // undefined
}
```

Ahora veamos cómo asignarle otro contexto:

```javascript
const uzi = {
  name: 'uzi',
  lastName: 'rodriguez',
}
function greeting() {
  console.log(`Hi, my name is ${this.name}`) // undefined
}

const greetUzi = greeting.bind(uzi) // Hi, my name is uzi
```

La función `bind` *ata* un contexto a una función. Otro ejemplo sería pasarle este mismo contexto a la función greeting dentro de un timeout:

```javascript
...

setTimeout(greeting.bind(uzi), 1000) // Hi, my name is uzi (after 1 sec)
```

La función `bind`, recibe siempre como primer argumento el contexto que se le quiera atar, posteriormente puede recibir n argumentos que tenga la función. Esta función solo genera una nueva función con un contexto cambiado, pero si queremos llamarla inmediatamente, usaremos `call`:

```javascript
...

greeting.call(uzi) // Hi, my name is uzi
```

La función `call`también recibe como primer argumento el contexto y como siguientes argumentos los que tenga la función. Existe  también la función `apply`, que hace lo mismo que `call`, salvo que no recibe n parámetros después del contexto, sino que los recibirá en forma de array:

```javascript
fn.apply(context, [arg1, arg2, ..., argn]) // Hi, my name is uzi
```

# ¿Cuándo hace falta poner punto y coma?

Realmente, usar punto y coma en JS es opcional, pero existen sus excepciones:

* Si empezamos una nueva línea como si escribiéramos un array:
  ```javascript
  someFunctionExecution();
  [1,2,3].map(forEach(n => console.log(n * 2)))
  ```
* Si empezamos una línea como si escribiéramos un template string:
  ```javascript
  someFunctionExecution();
  `${var} text ${var}`
  ```

Existe un caso en el que JS nos arrojará un error si tomamos los saltos de línea y punto y coma a la ligera:
```javascript
function someReturn() {
  return
  {
    k: 'v'
  }
}
```

En este caso, obtendremos un error, debido a que después del return, se considera que hay un ;

# ES6

ECMAScript es una especificación propuesta por ECMA, que es una organización de estándares. ES6, fue un estándar lanzado en 2015 que permite agregar características nuevas a JavaScript, a partir de ahí, cada año ha salido una nueva versión de ECMAScript.

## Default params
Nos permite agregar parámetros por defecto a una función para no tener que validarlos dentro de la misma:

```javascript
// Antes
function newFunction(name, age, country) {
  var name = name || 'uzi'
  var age = age || 23
  var country = country || 'MX'
  console.log(name, age, country);
}
// ES6
function newFunction(name = 'uzi', age = 23, country = 'MX') {
  console.log(name, age, country);
}
```

## Template literals
Nos permite unir varios elementos en un string:

```javascript
// Antes
let hello = 'hello'
let world = 'world'
let phrase = hello + ' ' + world
console.log(phrase)
// ES6
console.log(`${hello} ${world}`)
```

## Strings Multilínea
Los backticks, también nos permiten hacer textos multilínea:

```javascript
// Antes
let lorem = 'lorem ipsum dolor siamet commodi vel dius yet minima\n'
+ 'otra linea'
// ES6
let lorem2 = `lorem ipsum dolor siamet commodi vel dius yet minima
ahora es otra línea
`;
```

## Destructuración de objetos
Nos permite traer hacia un contexto, solo los elementos que necesitamos de un objeto:

```javascript
let obj = {
  name: 'Uzi',
  age: 23,
  country: 'MX'
}
// Antes
console.log(obj.name, obj.age, obj.country)
// ES6
let { name, age } = obj
console.log(name, age)
```


## Spread operator

Nos permite expandir objetos dentro de otros de una manera muy sencilla para unirlos:
```javascript
let team1 = ['Person 1', 'Person 2', 'Person 3']
let team2 = ['Person 4', 'Person 5', 'Person 6']
let education = ['Person 7', ...team1, ...team2]
```

## Let y Const

* **Let.** Es una nueva forma para declarar nuestras variables, a diferencia de *var*, solo está disponible en el scope en el que es declarado.
* **Const.** Es lo mismo que let, pero no se le puede reasignar un valor.

> siempre es preferente usar let y const dado que por el alcance de var, puede habre cruce de variables de maneras inesperadas y eso puede ocasionar bugs

## Fechas
Hagamos un programa que nos permita saber cuántos días tiene que nacimos para identificar el manejo de fechas en JS:

```javascript
// calc diff between dates in ms
function daysBetweenDates(date1, date2) {
  const msInOneDay = 1000 * 60 * 60 * 24
  // absolute value
  const diff = Math.abs(date1 - date2)
  return Math.floor(diff/msInOneDay)
}

const today = new Date()
const birth = new Date(1997, 5, 3)

daysBetweenDates(today, birth)
```

## Parámetros en objetos
Podemos llenar un objeto como elementos llave-valor (variable-valor) sin necesidad de especificar toda su estructura:
```javascript
let name = 'uzi'
let age = 23
obj = {name, age}
```

## [Arrow functions](https://github.com/uuzii/my-notepad/blob/main/engineering/js.md#arrow-functions)

## Promises
Nos ahorran escribir funciones como parámetros (callbacks) evitándonos la posibilidad de escribir un código horizontal:
```javascript
const helloPromise = () => {
  return new Promise((resolve, reject) => {
    if(true) {
      resolve('Promesa resuelta')
    } else {
      reject('Promesa fallida')
    }
  })
}
helloPromise()
  .then(response => console.log(response))
  .catch(error => console.error(error))
```

## [Clases](https://github.com/uuzii/my-notepad/blob/main/engineering/js.md#classes)

## Módulos
Son archivos externos a nuestro código en los que podremos separar algunos elementos de nuestro programa:

```javascript
const mod = () => {
  return 'hello'
}
export default mod
// Another js file...
import { mod } from './module';
```

## Generators
Es una función especial que retorna determinados valores según un algoritmo definido
```javascript
function* helloWorld() {
  if(true) {
    yield 'Hello, '
  }
  if(true) {
    yield 'World'
  }
}
const generatorHello = helloWorld();
console.log(generatorHello.next().value) // Hello,
console.log(generatorHello.next().value) // World
console.log(generatorHello.next().value) // undefined
```

# ES7

Es la versión de ECMA que fue lanzada en 2016, contiene algunas herramientas cómo:

## Includes
Nos permite saber si un elemento se encuentra en un array

```javascript
let numbers = [1,2,3,4,5,6,7]
if(numbers.includes(7)) {
  console.log('Sí se encuentra el 7')
} else {
  console.log('No se encuentra el 7)
}
```

## Exponent
Nos permite elevar una base a un determinado exponente

```javascript
let base = 4
let exponent = 3
let result = base ** exponent
```