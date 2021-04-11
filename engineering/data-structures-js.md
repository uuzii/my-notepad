# Estructuras de datos
Para empezar a entender las estructuras de datos, pensemos en un puñado de ropa completamente desordenada la cual luego tenemos que ordenar en su lugar. Para esto, podríamos amontonarla en el ropero pero quizá nosotros seríamos los únicos que la encontráramos, pero existen maneras de organizar la ropa como colgarla en ganchos, doblarla de ciertas maneras, lo cuál nos permitirá encontrarla más fácilmente y eventualmente nos ayudará a ahorrar espacio.

Esto mismo sucede en la programación, existen diferentes formas de guardar información, cada una de ellas sirve para resolver problemas distintos. En este caso, JavaScript per se, no posee todas las estructuras pero las crearemos.

> Las estructuras de datos son colecciones de valores, las relaciones entre ellos y funciones u operaciones que se pueden aplicar a los datos.

# Almacenamiento de los datos en la memoria
Imaginemos la memoria como una matriz en la que cada espacio denominaremos como *slot*. Cuando nosotros almacenamos un dato, sea un número o caracter, dicho dato requiere cierto número de slots (uno para cada bit). La primera posición o apuntador, es la dirección de nuestro dato en la memoria, ésta se utilizará cada vez que queramos accederla. Por otro lado, es importante decir que los slots en memoria se signan de manera random.

# Arrays
Son una colección de datos, cada una de ellas posee un índice y un valor. A los arrays se les conoce como *listas* en otros lenguajes de programación. en JS, existen ciertos métodos en el lenguaje para poder tratarlos, ejemplo:
* push. Agregar un elemento al final.
* pop. Borra el útlimo elemento.
* unshift. Agrega un elemento al inicio.
* shift. Borra el primer elemento.
* splice. Agrega un elemento en alguna parte del array.

Es importante que sepamos que existen dos formas de arrays:
* Estáticos
* Dinámicos

JavaScript define por default arrays **dinámicos**, pues puede tener n elementos, en otros lenguajes como C, se tiene que definir el tamaño (slots que ocupará en memoria) y este no puede mutar en tiempo de ejecución. Los arrays dinámicos, de inicio generan los slots **2x veces**. Es importante entender esto para no hacer crecer tanto los arrays pues siempre se previenen con el doble de memoria para crecer.

## Construyendo un array
Sabemos que los arrays ya existen el JavaScript, pero ahora construiremos uno con sus métodos mediante una clase:
```javascript
class MyArray {
  constructor() {
    this.length = 0;
    this.data = {}
  }

  get(index) {
    return this.data[index]
  }

  push(item) {
    this.data[this.length] = item;
    this.length++;
    return this.data;
  }
}

const myArray = new MyArray();
```

Donde el método `get`nos permitirá obtener un elemento mediante su posición y el método `push` nos permitirá agregar un elemento a la data de nuestro array. Estos elementos serán indexados mediante un número que es su índice y cada que se agregue uno se aumentará la longitud.

Agregaremos dos métodos más:
```javascript
  pop() {
    const lastItem = this.data[this.length - 1];
    delete this.data[this.length - 1];
    this.length--;
    return lastItem;
  }

  delete(index) {
    const item = this.data[index];
    this.shiftIndex(index);
    return item;
  }

  shiftIndex(index) {
    for(let i=index; i<this.length - 1; i++) {
      this.data[i] = this.data[i+1];
    }
    delete this.data[this.length - 1];
    this.length--;
  }
```

Donde `pop`nos permitirá eliminar el último elemento y `delete` nos permitirá eliminar un elemento puntual, nótese que está implementando otro método llamado `shiftIndex`, pues es necesario recorrer los índices de los elementos siguientes cuando elminamos un elemento enmedio de nuestro array.

Adicional, podríamos agregar má método propios del array, en este caso `shift`y `unshift`, estos para eliminar y agregar un elemento al incio respectivamente:
```javascript
  shift() {
    this.delete(0);
  }

  unshift(item) {
    for(let i=0; i<this.length; i++) {
      this.data[i+1] = this.data[i];
    }
    this.data[0] = item;
    this.length++;
  }
```

