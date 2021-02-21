🧮 v0.1.0

# Algoritmos

¿Qué entiende una computadora?

Las computadoras, son herramientas creadas por los seres humanos, al igual que muchas otras, para facilitar el trabajo de los seres humanos. En un inicio, se utilizaron para hacer cálculos matemáticos, hoy en día, se utilizan también para almacenar imágenes, audio, video y procesarlo, entre muchas otras actividades. Pero a bajo nivel, toda esta información está representada por *bits*, que es la presencia o la ausencia de energía en un semiconductor. Dado que estos dispositivos solo pueden emplear apagado/encendido, solo hay dos valores posibles (0/1) por esta razón, las computadoras solo emplean números **binarios**. ¿Cómo hacen para representar letras? Existe un código denominado ASCII, que es una tabla en la que se presentan las equivalencias de caractéres a un número binario. Lo mismo existe para las imágenes, videos, audio, etc.

Los algoritmos, son los pasos que sigue la computadora para procesar todos estos datos. Generalmente esto se hace mediante lenguajes de programación, éstos son sets de instrucciones para traducir nuestras indicaciones a lenguaje que entienda la máquina (en 1s y 0s). Uno de los primeros lenguajes que se usó fue *Assembler*, que con una serie de palabras conocidas como Mnemónicos, le podíamos mandar indicaciones directamente a la CPU. Existen otros lenguajes de nivel más alto como C, C++, posteriormente Java, Python y Javascript, en los que es más humana la forma de escribir, pero requieren mayor procesamiento.

  > ## Algoritmo
  > Conjunto de instrucciones que resuelven un problema dado paso a paso y sin generar ambigüedades.

## Estructuras de datos

Son una forma estructurada para trabajar con la información. Muchas de estas formas ya vienen incluídas en los lenguajes. Existen dos tipos:
* Lineales. Son estructuras secuenciales como los arrays.
* No lineales. Como los árboles o los grafos, que son puntos dispersos de información.

## Metodología para construcción de un algoritmo

Para elaborar un algoritmo, podemos seguir estos pasos:
1. Definición del problema. Identificar la problemática a resolver.
2. Análisis del problema. Comenzar a pensar qué necesitamos hacer y cuál es el resultado esperado.
3. Diseño del algoritmo. Lo escribimos, hacemos diagramas de flujo, programamos.
4. Verificación o pruebas. Planteamos los resultados esperados para poner a prueba los resultados de la computadora.

## Datos a extraer de un problema

* Entrada ¿Qué sse necesita para realizar los pasos?
* Salida ¿Qué se obtiene al final del algoritmo?
* Tipos de datos: números, texto, booleanos, otros.

## Variables

Una variable, la identificamos como una localidad en memoria que servirá para almacenar un dato en el tiempo, estos generalmente se clasifican en ciertos tipos, aquí veremos dos tipos de datos:

* **User defined data types.** Son aquellos que, como usuarios de un lenguaje de programación, definimos como tipos de datos (en C una struct o en POO una Clase). Ejemplo (En C):
  ```c
  #include <stdio.h>
  #include <stdlib.h>
  #include <string.h>

  struct client
  {
    char name[50];
    char id[10];
    float credit;
    char address[100];
  };

  int main()
  {
    struct client client1 = {0};
    strcpy(client1.name, "Uzi");
    strcpy(client.id, "0000001");
    client1.credit = 100000;
    strcpy(client1.address, "Calle 1");

    printf("The client name is %s", client.name);

    return 0;
  }
  ```
* **Abstract data types.** Un tipo de dato abstracto, representa un set particular de comportamientos, es decir, almacena datos, pero también el comportamiento de ellos. 
* Ejemplo: un stack que implementa el comportamiento *LIFO*. Otros ejemplos de este tipo: Queue, Priority Queue, Diccionarios, Graphs.
* Ejemplo: una estructura de datos linkeada como un *Array*. Otros ejemplos de este tipo: Linked lists, hash tables, trees.

Una descripción más detallada de los ADT más importantes...
* List. Utilizado para representar un número **definido** de valores ordenados, el mismo valor puede existir más de una vez, sería una implementación computacional del concepto matemático *secuencia finita*. Tiene índices seriados y posee acciones como get, insert, remove, remove at, replace, size, is empty, is full.
* Stack. Tiene tamaño definido y sirve como colección de elementos con dos operaciones principales: push y pop. Generalmente los conocemos como arrays linked lists. Posee la lógica **LIFO**, es decir que el primero en salir es el último que entró, tiene aotras acciones como peek, size, is empty, is full.
* Queue. Poseen la lógica **FIFO**, es decir que el primero en salir es el primero que entró. Tienen un tamaño definido. Sus acciones son: enqueue, dequeue, peek, size, is empy, is full.
* Linked list. A diferencia del array, que tiene tamaño definido, se maneja a través de nodos, mismos que a su vez se componen del valor y de un apuntador a la dirección del siguiente elemento.
* Dictionaries o Maps. Almacenan el valor y el índice, su ventaja es que nos permiten hacer consultas de manera más óptima.

### Creando una queue: arrays
Planteemos el siguiente problema: tenemos un restaurante con capacidad máxima para cinco comensales en el que necesitamos llevar el orden en el que se debe atenderles: primero a la primera persona que llega y luego a la segunda y así sucesivamente.

![queue](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/queue.jpg?raw=true)

Para implementarlo, usaremos el siguiente algoritmo:
1. Crear el pointer de nuestro *front* y nuestro *rear* (tomando en cuenta que hay un número máximo de elementos).
2. Colocar los valores de *front* y *rear* en `-1` pues no se ha agragado nada al queue (sería erróneo colocarle 0 pues siempre 0 representa la primera posición).
3. Agregar el valor de 0 al front al hacer el primer enqueue.
4. Incrementar en 1 el valor del *rear* cuando agreguemos un elemento.
5. Retornar el valor de *front* al quitar un elemento e incrementar en 1 el valor del *front* al usar un dequeue.
6. Antes de agregar un elemento, revisar si hay espacio.
7. Antes de remover un elemento, revisamos que existan elementos.
8. Asegurarnos de que al remover todos los elementos resetear nuestro *front* y nuestro *rear*.

La siguiente sería su implementación en C:
```c
#include <stdio.h>
#define SIZE 5
int values[SIZE], front = -1, rear = -1;

void printQueue() {
  for(int i=0; i<= rear; i++) {
    printf("[%d] ", values[i]);
  }
  printf("\n");
}
// la función enqueue recibe como argumento el valor que se agregará
void enQueue(int value) {
  // validamos que el queue no esté lleno
  if((rear - front) == SIZE)
    printf("Nuestro queue está lleno\n");
  else {
    // caso de uso del primer enqueue
    if(front == -1)
      front = 0;
    rear++;
    // inserción del valor
    values[rear] = value;
    printf("Se inseró el valor %d correctamente\n", value);
    printQueue();
  }
}
// no recibe ningún argumento pues solo sigue FIFO
void deQueue(){
  // caso de uso del queue vacío
  if(front == -1) {
    printf("El queue está vacío\n");
    printQueue();
  }
  else {
    printf("Se eliminó el valor %d\n", values[front]);
    printQueue();
    front++;
    // caso de uso de que ya eliminamos todos los elementos
    if(front > rear) {
      front = rear = -1;
      printQueue();
    }
  }
}
int main() {
  enQueue(1);
  enQueue(2);
  enQueue(3);
  enQueue(4);
  enQueue(5);
  deQueue();
  enQueue(10);
  return 0;
}
```

# Algoritmos de ordenamiento
Existen muchos algoritmos de ordenamiento, pero los principales conocidos son:
* Insertion sort
* Binary sort
* Bubble sort
* Quick sort
* Merge sort

Los cuáles entre más complejos son, generalmente son más eficientes.

> Un algoritmo de ordenamiento es un algoritmo que colocará objetos (números o letras) en orden (ascendente o desdcendente)

Ejemplo: ´[a,b,c,d,e,f]´ es un array ordenado alfabéticamente.

* **Merge sort**. Utiliza un principio conocido como *divide y vencerás*, en el cuál se separan conjuntos de datos, se resuelve el micro problema y posteriormente vamos mergeando en una lista principal. No conviene usarlo con arreglos pequeños, pero con arreglos grandes toma relevancia.
* **Insertion sort**. Comparamos pares de valores, y reubicamos el valor más pequeño al principio. Conviene cuando el set de datos es corto.
* **Bubble sort**. 
* **Quick sort**. Utiliza también el paradigma *divide y vencerás* pero de manera más eficiente. Es uno de los más rápidos.

Tomemos en cuenta la siguiente tipología:
* `S`: secuencia de objetos rdenables
* `N`: núm de elementos en S

## Bubble sort
Es el algoritmo más simple de todos los que hay: va tomando pares de datos y los cambia de lugar según el orden deseado, este proceso se repite hasta que está completamente ordenado, no conviene con sets de datos muy grandes.

![bubble-sort](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/bubble-sort-diagram.jpg?raw=true)

Trasladando esto a código, escribimos primeramente nuestros pasos:
1. Comparo y swapeo los elementos adyacentes
2. Repito hasta tener un recorrido completo sin ningún swap

Implementando el código en C:
```c
#include <stdio.h>

// Función que haga el swap
void change_position(int *n1, int *n2)
{
  // almacenará el dato de la posición mientras cambiamos el dato
  int temp = *n1;
  *n1 = *n2 ;
  *n2 = temp;
}

// función que haga el recorrido del array e implemente las comparaciones
void bubble_sort(int input_set[], int n)
{
  int i, j;
  // ciclos totales
  for(i = 0; i < n-1; i++)
  {
    // comparaciones en cada ciclo
    for(j = 0; j < n-i-1; j++)
    {
      if(input_set[j] > input_set[j+1])
        change_position(&input_set[j], &input_set[j+1]);
    }
  }
}

// Función que imprima el arreglo ordenado
void print_array(int input_set[], int n)
{
  int i;
  printf("[ ");
  for(i = 0; i < n-1; i++)
    printf("%d ,", input_set[i]);
  printf(" ]\n");
}

int main() {
  int input_set[] = {100, 1992, 0, 5, -1, 60, 70, 15, 14};
  int n = sizeof(input_set) / sizeof(input_set[0]);
  bubble_sort(input_set, n);
  print_array(input_set, n);
  printf("\n");
  return 0;
}
```
