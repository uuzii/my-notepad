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

