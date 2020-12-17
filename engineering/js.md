# Funciones declarativas y expresivas
* Las funciones declarativas, son aquellas que se declaran con la palabra reservada `function`
* Las funciones expresivas, son aquellas funciones anónimas que se guardan en una variable.

La diferencia entre ambas, es que a las funciones declarativas se les aplica hoisting, y a la expresión de función, no. Ya que el hoisting solo se aplica en las palabras reservadas var y function.

Lo que quiere decir que con las funciones declarativas, podemos mandar llamar la función antes de que ésta sea declarada, y con la expresión de función, no, tendríamos que declararla primero, y después mandarla llamar.

# Scope
Tradicionalmente se dice que es el alcance que tienen las variables, significa que, dependiendo dónde declaramos una función o variable, se definirá en qué lugar se pueden utilizar.

Los tipos de scope son:
* Scope Global. Es el ámbito global de todo nuestro código de Javascript
* Scoper Local. Se delimita por un *bloque*, que bien puede ser el contenido de una función
Veamos el siguiente fragmento:
  > `var name = "Uzi"
  > function printName() {
  >   var lastName = "Rodriguez"
  >   console.log(name + " " + lastName);
  > }`
En este ejemplo, es posible acceder desde un scope local (delimitado por la función, a un scope global, mas no es posible acceder desde el scope global al local).

# Hoisting
Es cuando las variables y las funciones se declaran antes que se procese cualquier otra parte del código. Esto sucede en versiones iguales o menor a *ECMAScript 5* ya que solo aplica a las palabras reservadas `var` y `function`; a partir de ECMAScript 6 se introdujo `let`y `const, que son formas de declarar variables que nos evitarán esto.

  > `console.log(name);
  > var name = 'Uzi' `
En el programa anterior, veremos un `undefined`, dado que el navegador no identificó esta varibale, aquí sucede el fenómeno *hoisting*, que se debe a que, en el *JIT compiler* al llegar al console log, se prepara un espacio en memoria para la variable pero no tiene un valor que asignarle.

En el caso de las funciones, podemos ejecutarlas antes que éstas se declaren, puesto que son lo primero que se identifica en el *JIT compiler*, incluso antes que las variables.

# Coerción
Muchas veces vemos en JavaScript suceden fenómenos como los siguientes:
  > `4 + '7' // '47'
  > 4 * '7' // 28`
Aquí sucede que, el lenguaje trata de ayudarnos a castear los datos de acuerdo a la operación que estamos tratando de hacer.

Hay dos tipos de coerción:
* Implícita. El lenguaje nos ayuda y cambiar de un tipo de valor a otro tipo de valor
* Explícita. Es cuando nosotros obligamos a que un tipo de valor se convierta a otro. Para hacer esto, podemos usar los métodos de datos primitivos, por ejemplo:
  > `String(20)`
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

# Prototype Array
Loa arrays son variables que alamacenan un conjunto de datos, algunos de sus métodos son:
* `Array.length`: muestra el número de elementos que tiene
* `Àrray[pos]`: muestra el elemento en la posición indicada
* `Array.push(element)`: agrega un elemento al final del array
* `Array.pop()`: eliminar el último elemento al final del array
* `Array.unshift(element)`: agrega un elemento al inicio del array
* `Array.shift()`: eliminar el primer elemento del array
* `Array.indexOf(element)`: muestra el index de un elemento en el array