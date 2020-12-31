🗿 v0.1.0

# Punteros

Para entenderlos, tenemos que entender cómo está hecha la memoria de la computadora, de antemano sabemos que son *tablas*, donde los datos se arreglan en conjuntos de 8 bits (1 byte). Esto significa que en un byte podemos guardar los números posibles de 0 a 255, si queremos guardar números más grandes, haremos uso de más bytes.

Fragmentación de la memoria. Cuanso se está usando el sistema, se utilizan espacios aleatorios en memoria, ordenarlos será tarea del sistema operativo, pero cada espacio en memoria, de cara a nosotros tendrá una posición en memoria. Declarar una variable, en esencia, consiste en eso, asignarle a la información un espacio en memoria.

> **Puntero.** Es una variable que puede guardar solamente direcciones de memoria, no información per se.

De este modo, si tenemos una variable **A**, cuya dirección de memoria es **1AF7**, y posteriormente guardamos en una variable **p**, dicha dirección, decimos que la variable p, *apunta* a A.

Traduciendo esto a código...

```c
int number = 1;
int * pointToNumber = &number;

printf("%p, %d\n", pointToNumber, *pointToNumber);
}
```

El símbolo de (&) hace referencia a la posición de memoria de una variable.

# Usos prácticos de los punteros

Veamos el siguiente programa, en donde se define una función que duplica el valor de una variable:

```c
void duplicar(int * a) {
  *a *= 2;
}

int main() {
  int a = 5;
  printf("Antes de duplicar: a = %d", a);
  duplicar(&a);
  printf("Después de duplicar: a = %d", a);

  return 0;
}
```

Esta situación, se conoce en otros lenguajes como *pasaje por referencia*, porque no estamos pasando a la función la variable en sí, sino solo una referencia a su lugar en memoria.

# Aritmética de punteros

Podemos hacer ciertas operaciones con los punteros:
* Incrementar
* Decrementar

Éstas nos permitirán acceder a ubicaciones de memoria distintas, por lo regular son válidas.

```c
int main() {
  int n = 5;
  int * pi = &n;
  char c = 'A';
  char * pc = &c;

  printf("Antes pi = %p y pc = %p", pi, pc);
  pi++;
  pc++;
  printf("Después pi = %p y pc = %p", pi, pc);

  return 0;
}
```

En este programa, veremos que se incrementan ciertas unidades dependiendo el tipo de variable hacia otras localidades de memoria.

Al hacer esto, debemos tener cuidado, pues podríamos pisar una localidad de memoria que esté en uso.

```c
int main() {
  int * pi;
  int a = 5;
  int b = 1;

  pi = &a;
  printf("Antes a = %d, &a = %p, b = %d, &b = %p, pi = %p\n", a, &a, b, &b, pi);
  pi++;
  *pi = 10;
  printf("Después a = %d, &a = %p, b = %d, &b = %p, pi = %p\n", a, &a, b, &b, pi);

  return 0;
}
```

En este ejemplo, vemos cómo la variable b es modificada, pues el puntero apuntó a ella y a través del puntero se le asignó otro valor. ¿Qué sentido tiene eso? Cuando tenemos control sobre las variables, como en los **Arreglos**

# Arrays

Son estructuras de datos muy simples que representan datos relacionados y son del mismo tipo, esta es la forma de definir un array y llenarlo:

```c
int main() {
  int array[5];
  int i;

  for(i=0; i<5; i++) {
    array[i] = i * 2;
  }

  for(i=0; i<5; i++) {
    printf("array[%d]: %d\n", i, array[i])
  }

  return 0;
}
```

Existe una relación muy cercana entre arreglos y punteros: *todos los arreglos son punteros, pero no todos los punteros son arreglos*. Cuando se define un arreglo, observamos las siguientes situaciones:
* Se reserva un espacio en la memoria para todos los elementos del arreglo
* Se crea un puntero
* Apuntando ese puntero a la dirección en memoria de la primera posición del arreglo

# Strings

En C, no hay un tipo de dato primitivo para cadenas de caracteres, pero se usa un puntero a una serie de caracteres, se definen de la siguiente manera:

```c
int main() {
  char * name = "Uzi";
  printf("Nombre: %s\n", name);

  return 0;
}
```

Vemos que se trata de un array de caracteres, aunque no le especificamos el tamaño, el compilador calcula por nosotros cuál será su tamaño en memoria. Si lo imprimimos de la siguiente manera, veremos cómo cada posición corresponde a una localidad de memoria:

```c
  for(int i=0; i<3; i++) {
    printf("name[%d](%p) = %c\n", i, name + i, name[i]);
  }
```

Otra forma de definir un string, como un array ilimitado es:

```c
int main() {
  char name[] = "Uzi Jonadab Rodriguez";

  printf("Mi nombre es %s\n", name);

  return 0;
}
```

Este programa nos muestra todo un string pero no henmos definido cuál es el límite de tamaño sino que, el compilador lo hace por nosotros. Los strings cuentan con un caracter especial para señalarles a otras funciones su final, este es el caractér *\0*.

Para utilizar las cadenas de mejor manera, existe la librería *<string.h>*, tiene herramientas como:
* `strlen(pointerToString)`: nos brinda la longitud de una cadena
* `strcmp(pointerToStr, pointerToStr)`: nos devuelve 0 si son iguales, -1 si la primera es menor que la segunda, 1 si la segunda es menor que la primera.
* `strcpy(destinationStr, sourceStr)`: Copia una cadena en otra que señalemos como destino.

# Parámetros de la función main desde línea de comandos

Nosotros podemos introducir argumentos a la función main desde línea de comandos con el `gcc`, si queremos visualizarlos, podemos hacer lo siguiente:

```c
#include <stdio.h>

int main(int argc, char * argv[]) {
  for(int i = 0; i < argc; i++) {
    printf("Argumento %d = %s\n", i, argv[i]);
  }

  return 0;
}
```

Donde:

* **argc:** es la cantidad de argumentos que recibe la función
* ***argv[]:** es un array de cadenas de caractéres que recive la función

De esta manera, el sistema operativo puede enviar parámetros a nuestra función main ejecutando lo siguiente:

```bash
gcc my_program.c -o my_program
./my_program
```

Veremos a la salida:

```bash
Argumento 0 = ./my_program
```

Dado que el SO siempre envía por defecto el nombre del programa, pero si intentamos ahora añadiendo otros argumentos, podremos verlos en pantalla:

```bash
./my_program arg1 arg2
Argumento 0 = ./my_program
Argumento 1 = arg1
Argumento 2 = arg2
```

Esta funcionalidad se puede prestar a un overflow, si tenemos que hacer algún tratamiento a los datos, usando un array limitado, el programa se colgará:

```c
int main(int argc, char * argv[]) {
  char buffer[20];
  char * buffer2 = "This will be overwritten";

  printf("Original buffer 2: %s\n", buffer2);
  strcpy(buffer, argv[1]);
  printf("New buffer2: %s\n", buffer 2);
}
```

Para evitar este problema, siempre hay que validar la calidad de los datos que nos ingresan a un programa antes de hacer cualquier tratamiento, ya que no hacerlo, hace a nuestros programas muy suceptibles a hackers.

# Datos estructurados (structs)

Constantemente se presentan casos en los que requerimos que los datos se estructuren de cierta manera, para ello es importante el uso de los *structs* en c. En el siguiente ejemplo, crearemos una estructura que se llenará con argumentos externos, mismos que serán validados para evitar un overflow:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char * argv[]) {
  struct {
    char name[100];
    int age;
  } person;

  if(argc < 3) {
    printf("Indique nombre y edad por favor\n");
    return 1;
  }

  if(strlen(argv[1]) < 100) {
    strcpy(person.name, argv[1]);
  }

  person.age = atoi(argv[2]);

  printf("Name: %s, age = %d\n", person.name, person.age);

  return 0;
}
```

## Punteros a estructuras

En el siguiente ejemplo, replicamos la aplicación anterior, pero utilizanod una estructura genérica como si fuera un *tipo de dato custom* y en lugaar de pasarlo directamente a la función (sillPersonData) que llena nuestra struct, lo pasamos por referencia.

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct PERSON {
  char name[100];
  int age;
};

void fillPersonData(struct PERSON * person, const char * name, int age) {
  if(strlen(name) < 100) {
    strcpy((*person).name, name);
  }
  (*person).age = age;
}

int main(int argc, char * argv[]) {
  struct PERSON person;

  if(argc < 3) {
    printf("Indique nombre y edad por favor\n");
    return 1;
  }

  fillPersonData(&person, argv[1], atoi(argv[2]));

  printf("Name: %s, age = %d\n", person.name, person.age);

  return 0;
}
```

Una forma más legible de escribir la función `fillPersonData`, es la sintáxis de *flecha*, que nos permite intuir de una mejor manera que queremos acceder a un elemento de una estructura a la que estamos apuntando:

```c
  void fillPersonData(struct PERSON * person, const char * name, int age) {
    if(strlen(name) < 100) {
      strcpy(person->name, name);
    }
    person->age = age;
  }
```

# Alias de tipos de datos

Constantemente se vuelve necesario definir tipos de datos que hagan mayor sentido conceptual que los datos primitivos, ejemplo: un número de teléfono generalmente será un string, pero tendrá más sentido para nosotros trabajarlo como *phone*. Definamos algunos tipos en nuestra aplicación anterior:

```c
...

typedef char NAME[100];
typedef int AGE;

struct PERSON {
  NAME name;
  AGE age;
};

...
```

Esto cumplirá la misma función, solo que ahora tendremos un alias para nuestros tipos de datos, sumando la ventaja de que cuando decidamos que los campos deben crecer, podemos modificar en un solo toque a toda la aplicación.

# Bibliotecas de funciones

Cuando nuestro código crece, podemos generar archivos externos que contengan lógica u otras partes de nuestra aplicación (a semejanza de stdio o stdlib). Las bibliotecas en C, se dividen en dos archivos:
* Encabezados. Contiene definición de funciones y constantes. Ejemplo:
  ```c
  // mylib.h

  #ifndef __MY_LIB_H
  #define __MY_LIB_H

  void hello(const char * name);
  #endif
  ```

  Donde la directiva *ifndef* nos permite validar que no exista otra biblioteca con el mismo nombre para poder definirla y usarla en otros programas.

* Implementación. Contiene el código que le dará sentido a las definiciones.
  ```c
  // mylib.c

  #include <stdio.h>
  #include "mylib.h"
  void hello(const char * name) {
    printf("Hola %s\n", name);
  }
  ´´´

  Nótese que no incluimos la librería con los signos de <>, sino que lo hacemos con comillas, esto se hace porqe es una librería creada por nosotros y la buscará en su propio directorio.

Para hacer uso de nuestra librería:

```c
// mylib_usage.c

#include <stdio.h>
#include <stdlib.h>
#include "mylib.h"

int main(int argc, const char * argv[]) {
  hello(argv[1]);
  
  return 0;
}

A la hora de hacer la compilación, tendremos que incluir también nuestra librería en el gcc para generer un único binario:

```bash
gcc mylib_usage.c mylib.c -o mylib_usage
```

# Manejo dinámico de la memoria

En muy pocos casos, sabemos realmente cuánta memoria estamos utilizando, por ello se vuelve necesario manejarlo de manera más dinámica, para ello utilizaremos las funciones:
* malloc
* realloc

que nos permitirán interactuar con el sistema operativo para reservar espacios contiguos en la memoria, veamos el siguiente ejemplo, que es una alicación para almacenar contactos:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef char NAME[100];
typedef char EMAIL[100];

typedef struct {
  NAME name;
  EMAIL email;
} CONTACT;

int main(int argc, char * argv[])
{
  CONTACT * list = NULL;
  char buffer[100];
  int goOn, listSize = 0;

  do {
    printf("Write the name of the new contact (0 to exit):\n");
    scanf("%99s", buffer);
    if(strcmp("0", buffer) != 0) {
      if(!list) {
        list = malloc(sizeof(CONTACT));
      } else {
        list = realloc(list, sizeof(CONTACT) * (listSize + 1));
      }

      strcpy(list[listSize].name, buffer);
      printf("Write the email of %s:\n", buffer);
      scanf("%99s", buffer);
      strcpy(list[listSize].email, buffer);
      goOn = 1;
      listSize++;
    } else {
      goOn = 0;
    }
  } while(goOn);

  printf("\nThis is your contacts list:\n");
  printf("Name\t\tEmail\n");

  for(int i = 0; i < listSize; i++) {
    printf("%s\t\t%s\n", list[i].name, list[i].email);
  }

  return 0;
}
```

Donde es importante notar los siguientes puntos:
* El puntero `list`, servirá como apuntador al primer contacto de tipo `CONTACT`
* `buffer`tendrá una capacidad máxima de 99 carácteres + \0 que marcaría el final del string
* La bandera `goOn` nos indicará si continuamos registrando contactos
* El número `listSize` será un contador de los contactos que llevamos registrados
* La validación `if(!list)` se encargará de validar qué función ejecutar para empezar a reservar memoria:
  * Si está vacía (!list), la función `malloc` reservará tanta memoria como sea necesaria para almacenar el tipo de dato *CONTACT*. Para determinar cuánto se necesita para ese tipo de dato, se usa la función `sizeof(type)`
  * Si la lista no está vacía, la función `realloc`, cumple un objetivo similar, pero toma el espacio reservado y *lo agranda*, es decir, que reserva un nuevo espacio a partir de donde topó la reservación anterior.

Con esto, lograremos reservar el espacio adecuado en ubicaciones contiguas de la memoria para poder presentarle al usuario la aplicación de registro de contactos.

# Memory leaks y Garbage collectors

Después de haber reservado espacio en memoria a través de `malloc` y `realloc`, dicho espacio en memoria queda perdido, a este fenómeno se le conoce como *memory leak*. En otros lenguajes existen los denominados *garbage collectors* para liberar dicho espacio, en C, este proceso lo haremos de manera manual; para el ejemplo de la lista de contactos, haríamos lo siguiente:

```c
  ... 
  for(int i = 0; i < listSize; i++) {
    printf("%s\t\t%s\n", list[i].name, list[i].email);
  }

  free(list);
  return 0;
}
```

Hay casos en los que, detectar estos leaks no es tan fácil, veamos otro ejemplo:

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
  char * p = NULL;
  for(int i = 0; i < 10; i++) {
    printf("%d Alocando 50 bits\n");
    if(p) {
      free(p);
    }
    p = mlloc(sizeof(char)*50);
  }

  printf("Terminado!\n");
  free(p);
}
```

Notar lo siguiente:
* Estamos generando un puntero a `NULL` al inicio
* En cada iteración, validamos si `p` ya tiene algún valor con la finalidad de liberarla
* Al final, liberamos lo que haya quedado en `p``

Una herramienta que nos puede ayudar, es **valgrind**, misma que ejecutamos con nuestro binario de la siguiente manera:

```bash
$ valgrind ./[binaryFile]
```

Para ver un informe de cuánta memoria se ha reservado y/o liberado.

# Listas enlazadas

Tanto en reservas estáticas como dinámicas, hemos usado espacios contiguos. Pero a medida que nuestros programas crecen, es importante organizar la memoria para compactar lo más posible el uso de la memoria.

La solución para esto, son las *listas enlazadas*, que no necesariamente requieren de usar espacios contiguos, sino que harán un encadenamiento de elementos conocidos como *nodos*. Básicamente, consiste en almacenar el valor de una lista junto con su dirección en memoria, el siguiente elemento de la lista, será dicha dirección + el tamaño del nodo anterior y así sucesivamente. En este programa, se hace un enlace de nodos:

```c
#include <stdio.h>
#include <stdlib.h>
// Definir un nodo
typedef struct Node {
  int number; // lista
  struct Node * next; // estructura autoreferenciada
} NODE;
// función que crea nodos
NODE * createNode(int number) {
  NODE * newNode;
  newNode = malloc(sizeof(NODE));
  newNode->next = NULL; // por default el siguiente nodo será NULL
  newNode->number = number; // asignar el argumento a number
  return newNode;
}
int main(int argc, char * argv[]) {
  // creando una lista
  NODE * start = NULL, * current;
  // bandera que nos indicará si continuamos
  char goOn;
  // tamaño de la lista
  int listSize = 0, number;
  // pedimos inputs al usuario
  do {
    printf("La lista contiene %d nodos, Ingrese el siguiente numero (0 para finalizar)\n", listSize);
    scanf("%d", &number);
    // si el número es distinto de cero...
    if (number) {
      // si start es igual a NULL, crea el nodo
      if(!start) {
        start = createNode(number);
        printf("Primer nodo start->number %d\n", start->number);
        printf("Primer nodo start->next %p\n", start->next);
        listSize++;
      } else { // si start no es igual a NULL...
        current = start; // usamos current como auxiliar para no perder el inicio
        printf("Primer nodo start->number %d\n", start->number);
        printf("Primer nodo start->next %p\n", start->next);
        // mientras current tenga algo en next:
        while(current->next) {
          printf("while antes de asignar current->number %d\n", current->number);
          printf("while antes de asignar current->next %p\n", current->next);
          current = current->next;
          printf("while después de asignar current->number %d\n", current->number);
          printf("while después de asignar current->next %p\n", current->next);
        }
        // se crea el siguiente nodo
        current->next = createNode(number);
        listSize++;
      }
      goOn = 1;
    } else {
      goOn = 0;
    }  
  } while(goOn);
  // inicia current con el start
  current = start;
  printf("Inicializa current->number %d\n", current->number);
  printf("Inicializa current->next %p\n", current->next);
  printf("La lista tiene los numeros: \n");
  // mientras current no sea NULL
  while(current) {
    // desreferencía current y accede a number
    printf("%d %p", current->number, current->next);
    // si existe un siguiente...
    printf(current->next ? ", " : " \n");
    // para iterar
    current = current->next;
  }
  return 0;
}
```

## Liberar espacio de una lista enlazada

Para liberar el espacio utilizado en listas enlazadas, podemos hacer lo siguiente a nuestro programa anterior a partir del final:

```c
  NODE * start = NULL, * current, * next;
...
  current = start;
  printf("Inicializa current->number %d\n", current->number);
  printf("Inicializa current->next %p\n", current->next);
  printf("La lista tiene los numeros: \n");
  // mientras current no sea NULL
  while(current) {
    // desreferencía current y accede a number
    printf("%d %p", current->number, current->next);
    // si existe un siguiente...
    printf(current->next ? ", " : " \n");
    // para iterar
    current = current->next;
  }

  current = start;
  while(current) {
    next = current->next;
    free(current);
    current = next
  }
  return 0;
}
```

Otras estrucutras avanzadas que se pueden implementar en C, son:
* Listas doblemente enlazadas
* Listas circulares
* Áboles binarios
* Grafos

# Cálculos complejos
Existe una librería que nos permite hacer cálculos complejos: `Math.h`. En el siguiente ejemplo, hacemos un programa que calcula la hipotenusa de un triángulo, dados como argumentos principales del programa lado a y lado b:
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>

int main(int argc, char * argv[]) {
  int a, b;
  a = atoi(argv[1]);
  b = atoi(argv[2]);
  printf("La hipotenusa mide %.2f\n", sqrt(pow(a,2) + pow(b,2)));
  return 0;
}
```

Para generar números de manera *aleatoria* tenemos la biblioteca `time`:
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
  srand(time(NULL));
  printf("%d\n", rand());
  return 0;
}
```

`time` también nos ayuda para realizar cálculos de fechas:
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main(int argc, char * argv[]) {
  // time_t es una estructura de la librería time
  // today es el tiempo en segundos desde el segundo 0 de unix
  time_t oneDay = 24 * 60 * 60, today = time(NULL), nextDate;
  nextDate = today + oneDay * atoi[argv[1]];
  // ctime permite tomar una estructura de tipo time_t a un string formateado a fecha
  printf("%s\n", ctime(&nextDate));
  return 0;
}
```

# Manejo de archivos de texto
Veremos cómo abrir y guardar archivos de texto. Para leerlos:
```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char * argv[]) {
  /**
    FILE, de stdlib, permite trabajar con archivos del sistema de archivos
    fopen() hace un llamado al sistema operativo para levantarlo en memoria
    y abrirlo, recibe el nombre del archivo y el modo en el que lo queremos
    abrir
   */
  FILE * fp = fopen(argv[1], "r");
  char c;
  /**
   * fgetc obtiene un caracter de un archivo abierto
   * EOF es una constante que marca el fin del archivo (end of file)
   */ 
  while((c = fgetc(fp)) != EOF) {
    printf("%c", c);
  }
  // liberamos los recursos que el archivo haya usado
  fclose(fp);
  return 0;
}
```

Para escribir un archivo de texto:
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char * argv[]) {
  FILE * out;
  char buffer[100];
  // Si el archivo no se puede abrir por alguna razón
  if(!(out = fopen(argv[1], "w"))) {
    printf("No puedo escribir el archivo %s\n", argv[1]);
    return -1;
  }
  do {
    printf("Ingrese una palabra:");
    scanf("%99s", buffer);
    fputs(buffer, out);
    fputs("\n", out);
    printf("Grabada! Otra? (s/n):");
    scanf("%1s", buffer);
  } while(buffer[0] != 'n');
  // es muy importante cerrar el archivo antes de salir
  fclose(out);
  return 0;
}
````

En este caso ejecutaremos con:
```bash
$ ./file [destination file]
```
Y el programa nos pedirá ir introduciendo palabras hasta que pulsemos n. Si queremos corroborar que ha quedadob bien escrito el archivo, usamos el programa anterior de lectura.

# Leer y escribir archivos binarios
Lo anterior también lo podemos hacer para archivos binarios:

Ecritura (el programa requerirá como argumento el nombre del archivo que queremos escribir):
```c
#include <stdio.h>
#include <stdlib.h>

struct person {
  char name[100];
  unsigned int age;
};

int main(int argc, char * argv[]) {
  FILE * out;
  char buffer[100];
  // Si el progrma no recibe el argumento:
  if(argc < 2) {
    printf("Indique el nombre del archivo de salida\n");
    return -1;
  }
  // Si el alrchivo no se puede abrir
  if(!(out = fopen(argv[1], "w"))) {
    printf("No puedo escribir en el archivo %s\n", argv[1]);
    return -2;
  }
  struct person p;

  do {
    printf("Ingrese el nombre de la persona: ");
    scanf("%99s", p.name);
    printf("Ingrese la edad de la persona: ");
    scanf("%d", &(p.age));
    /**
     * fwrite escribe directamente en binario, recibe la dirección donde
     * queremos guardar, luego el tamaño de lo que quiero guardar, luego
     * cuántos queremos guardar, por último dónde queremos guardar
     */ 
    fwrite(&p, sizeof(struct person), 1, out);
    printf("Grabada! Otra?: (s/n): ");
    scanf("%1s", buffer);
  } while(buffer[0] != 'n');
  fclose(out);
  return 0;
}
```

Lectura (el programa requerirá como argumento el nombre del archivo que queremos leer):

```c
#include <stdio.h>
#include <stdlib.h>

struct person {
  char name[100];
  unsigned int age;
};

int main(int argc, char * argv[]) {
  FILE * in;
  char buffer[100];
  // Si el progrma no recibe el argumento:
  if(argc < 2) {
    printf("Indique el nombre del archivo de entrada\n");
    return -1;
  }
  // Si el alrchivo no se puede leer
  if(!(in = fopen(argv[1], "r"))) {
    printf("No puedo leer en el archivo %s\n", argv[1]);
    return -2;
  }
  struct person p;
  /**
     * fread lee directamente un binario, recibe la dirección donde
     * queremos leer, luego el tamaño de lo que quiero leer, luego
     * cuántos queremos leer, por último dónde queremos leer
     */ 
  while(fread(&p, sizeof(struct person), 1, in)) {
    printf("%s tiene %d años\n", p.name, p.age);
  }
  fclose(in);
  return 0;
}
```
