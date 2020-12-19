# Funciones declarativas y expresivas
* Las funciones declarativas, son aquellas que se declaran con la palabra reservada `function`
* Las funciones expresivas, son aquellas funciones anónimas que se guardan en una variable.

La diferencia entre ambas, es que a las funciones declarativas se les aplica hoisting, y a la expresión de función, no. Ya que el hoisting solo se aplica en las palabras reservadas var y function.

Lo que quiere decir que con las funciones declarativas, podemos mandar llamar la función antes de que ésta sea declarada, y con la expresión de función, no, tendríamos que declararla primero, y después mandarla llamar.

# Arrow functions
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

# Clases
Las clases en Js, no son iguales que en otros lenguajes, realmente son prototipos con *sugar syntax*. Si queremos, por ejemplo crear un prototipo (o función constructura) e instanciarlo, hacemos lo siguiente:
  ```javascript
    function Persona(name) {
      this.name = name
    }
    var person = new Persona('Uzi')
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