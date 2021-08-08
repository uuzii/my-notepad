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

# Strings
Los strings no son uns estructura de datos en sí, pero la forma en la que se guardan, tiene un mecanismo por el cual sí lo analizaremos como tal. en la mayoría de lenguajes de programación, como en JS, los strings son *inmutables*, lo cuál quiere decir que una vez creados ya no se pueden modificar, esto se debe a que cada caracter que forma los strings toma una posición de memoria (de manera similar a los arrays), entonces para manipularlos, hay que tomar cada posición de la memoria, buscar espacios nuevos disponibles o hacer distintos cómputos.

|Adress|Data|
|-|-|
|0|H|
|1|o|
|2|l|
|3|a|

# Hash tables
Las hash tables no vienen incluídas en Javascript, como en algunos otros lenguajes, aunque una buena aproximación son los objetos, a continuación, veremos cómo se denominan estas estrcturas en algunos lenguajes:

|Lenguaje|Equivalencia|
|-|-|
|JavaScript|Objetos|
|Python|Diccionarios|
|Java|Maps|
|Go|Maps|
|Ruby|Hashes|

Las has tables manejan, al igual que en los objetos de JS, el concepto de *llave-valor*, con la diferencia de que son indexadas en una estructura de una tabla llamada *bucket* con un índice denomindo *hash*. Dicho hash, es generado por una función que por ahora veremos como una caja negra llamada *hash function*:

### Buckets:
|Hash|Key|Value|
|-|-|-|
|234||6|
|235|||
|...|||
|237|Mandarinas|20|
|238|||
|...|||
|241|||

Donde cada hash, es generado por una *Hash function*, que siempre asociará el mismo hash a una misma llave dada, de esta manera será fácil de encontrar, como si fuera el index de un array; veámoslo como un objeto con un paso intermedio, en el que la llave se convertirá en un hash, mismo que será su ubicación.

Los métodos que utiliza esta estructura de datos son los siguientes:
|Método|Acción|
|-|-|
|insert|Insertar un elemento en la tabla|
|search|Buscar un elemento por key|
|delete|Borrar un elemento|

## Colisión de Hash table
En ocasiones, nuestra Hash function, puede generar el mismo hash para llaves distintas, con lo cuál se gardan los dos valores correspondientes en el mismo bucket. Es imposible evitar que existan estas colisiones, pero hay formas de tratar esta situación, por ejemplo, pasándole un key puntual de manera que solo nos regrese el valor correspondiente, de esta situación particular surge otra estructura de datos que son las *linked lists*.

A continuación veremos la implementación de hash tables en JS:
```javascript
class hashTable {
  constructor(size) {
    this.data = new Array(size);
  }
  /**
   * La usaremos de manera didáctica como hash function, pero
   * por lo regular usaremos funciones hash de terceros
   */
  hashMethod(key) {
    let hash = 0;
    for(let i=0; i<key.length; i++) {
      hash = (hash + key.charCodeAt(i) * i) % this.data.length;
    }
    return hash;
  }

  // Para agregar elementos a la hash table
  set(key, value) {
    const address = this.hashMethod(key);
    // Para almacenar nuevos elementos si no existe la dirección
    if(!this.data[address]) {
      this.data[address] = [];
    }
    // Para almacenar datos de colisiones
    this.data[address].push([key, value]);
    return this.data;
  }
}

// Generamos nuestra instancia con cierta cantidad de buckets
const myHashTable = new hashTable(50);

myHashTable.set("Uzi", 1997);
```

Hasta este punto, veremos que se agrega un elemento en una posición random de nuestro table, podemos seguir agregando y a veces habrá colisiones, pero en esos casos, la data se guardará en el mismo bucket.

Ahora implementaremos otras funciones para obtener un valor conforme a su key, para borrar un item y también para obtener todas las llaves:

```javascript
  get(key) {
    const address = this.hashMethod(key);
    // Obteniendo el índice o address del elemento buscado
    const currentBucket = this.data[address];
    // validamos que exista el bucket en nuestra tabla
    if (currentBucket) {
      for (let i=0; i<currentBucket.length; i++) {
        // buscamos nuestro elemento en el bucket
        if (currentBucket[i][0] === key) {
          return currentBucket[i][1];
        }
      }
      return undefined;
    }
  }

  deleteItem(key) {
    const address = this.hashMethod(key);
    // Obteniendo el índice o address del elemento buscado
    const currentBucket = this.data[address];
    // validamos que exista el bucket en nuestra tabla
    if (currentBucket) {
      for (let i=0; i<currentBucket.length; i++) {
        // buscamos nuestro elemento en el bucket
        if (currentBucket[i][0] === key) {
          delete currentBucket[i];
        }
      }
    }
  }

  getKeys() {
    const collection = [];
    for (let i=0; i<this.data.length; i++) {
      if (this.data[i] && this.data[i].length) {
        const subCollection = this.data[i].reduce((acum, current) => {
          acum.push(current[0])
          return acum;
        },  []);
        collection.push.apply(collection, subCollection);
      }
    }
    return collection;
  }
```

# Linked lists
Las linked lists no son muy similares a los arrays y a las estrucutras que hemos revisado, la direncia radica en que, cada elemento de la lista, está linkeado al anterior, de modo que hay una cabeza y una cola.

![linked list](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/linked-list.jpg?raw=true)

Existen dos tipos de listas enlazadas pero solo trataremos las *singly linked lists*. Existen ciertos métodos para manipular esta estructura:

|Método|Descripción|
|-|-|
|prepend|agregar un nodo al inicio|
|append|agregar un nodo al final|
|lookup/search|buscar un nodo|
|insert|insertar un nodo a la lista (entremedio)|
|delete|borrar un nodo|

Consideremos que en una linked list, cada elemento posee dos valores: el valor que contiene en sí, y el siguiente valor.

![singly linked list](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/singly-linked-list.jpg?raw=true)

Para encontrar un elemento en una lista de este tipo, tenemos que recorrer cada uno de los elementos de la lista, si lo hemos rebasado, será necesario reiniciar la búsqueda desde la cabeza.

## Consideraciones
Puede llegar a ser complejo encontrar nodos dentro de una estructura de datos de este tipo, pues no tenemos almcenado en memoria la dirección per se de los datos, sino que dicha dirección solo se encuentra en el valor anterior, por ello no han índices que nos permitan encontrar de manera rápida los elementos. Para representarlo, imaginemos que queremos generar la siguiente lista:

```txt
1 --> 2 --> 3 --> 4 --> null
```

Entonces, en JavaScript veríamos algo así:

```javascript
let singlyLinkedList = {
  head: {
    value: 1,
    next: {
      value: 2,
      next: {
        value: 3,
        next: {
          value: 4,
          next: null
        }
      }
    }
  }
}
```

Implementemos ahora esta estructura mediante una clase:
```javascript
class MySinglyLinkedList {
  // De manera obligada contará con un elemento que es la cabeza
  constructor(value) {
    this.head = {
      value: value,
      next: null
    }
    // la cola será a la vez la cabeza
    this.tail = this.head;
    // almacenaremos la longitud, que inicialmente sería 1
    this.length = 1;
  }
}

/**
 * Implementaremos también una clase *nodo*,
 * que nos servirá para agregar elementos de
 * manera rápida:
 */
class Node {
  constructor(value) {
    this.value = value;
    this.next = null
  }
}

// Instanciamos
const myLinkedList = new MySinglyLinkedList(1);
```

## Agregar nodos al final de la lista
Implementaremos dentro de nuestra clase un método `append` para agregar nodos al final de nuestra lista:

```javascript
append(value) {
  // generamos el nuevo nodo
  const newNode = new Node(value);
  // sustituímos el último null por nuestro nuevo nodo
  this.tail.next = newNode;
  // posicionamos nuestro nuevo nodo como cola
  this.tail = newNode;
  // incrementamos el tamaño de la lista
  this.length++;
  return this;
}
```

## Agregar nodos al inicio de la lista
Implementaremos dentro de nuestra clase un método `prepend` para agregar nodos al inicio de nuestra lista:

```javascript
prepend(value) {
  // generamos el nuevo nodo
  const newNode = new Node(value);
  // agregamos el antiguo head al next de nuestro nuevo nodo
  newNode.next = this.head;
  // posicionamos nuestro nuevo nodo como cabeza
  this.head = newNode;
  // incrementamos el tamaño de la lista
  this.length++;
  return this;
}
```

## Agregar nodos en una posición puntual
Implementaremos un método `insert` para agregar nodos en alguna posición de nuestra lista, para ello, tenemos que tomar en cuenta lo siguiente: para insertar un nodo enseguida de otro, tendríamos que desligar el que éste tiene como  *next*, de modo que podríamos generar una pérdida de información, por ello es importante prevenir que esto suceda y en el algoritmo describiremos una idea de cómo hacerlo.

> Disclaimer: esta solución no es la única que puede existir, pueden haber otras formas de llegar al mismo resultado,

Algoritmo:
* Tenemos que recorrer toda la lista hasta encontrar el elemento tras el cual queremos añadir el nuevo elemento
* Una vez hallado, hay que hacer que nuestro nuevo nodo, apunte al que solía apuntar el nodo encontrado
* Finalmente, apuntaremos el nodo encontrado al nuevo nodo, en el código implementaríamos esta solución:

```javascript
  insert(index, value) {
    // consideremos que la lista pueede estar vacía y no tendría caso hacer la búsqueda del índice
    if (index >= this.length) {
      return this.append(value);
    }
    // generamos el nuevo nodo
    const newNode = new Node(value);
    // obtenemos el índice del elemento en la posición anterior
    const prevItem = this.getIndex(index - 1);
    // generamos una variable temporal para que el dato que solía estar en next no se pierda
    const tempHolding = prevItem.next;
    // sustituímos el next de la posición encontrada con el nuevo nodo
    prevItem.next = newNode;
    // colocamos el elemento on-holding como next de nuestro elemento recién insertado
    newNode.next = tempHolding;
    this.length++;
    return this;
  }

  getIndex(index) {
    let counter = 0;
    let currentNode = this.head;
    while (counter !== index) {
      currentNode = currentNode.next;
      counter++;
    }
    return currentNode;
  }
```

## Remover  nodos en una posición puntual
Para eliminar un nodo puntual, usaremos una lógica muy parecida a la anterior, reutilizando el método `getIndex` para ubicarnos en una posición anterior a la que queremos eliminar:

```javascript
  remove(index) {
    if (index < 0) {
      console.error('Posición inválida, intenta otra.');
    } else if (this.length === 1 && index === 0) {
      console.error('Solo hay un elemento en la lista y no lo puedes eliminar.');
    } else {
      const prevItem = this.getIndex(index - 1);
      prevItem.next = prevItem.next.next;
      this.length--;
    }
    return this;
  }
```