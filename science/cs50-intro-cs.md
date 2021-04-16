# Computer Science

¿Qué es exactamente la ciencia computacional?
Pensemos en este proceso:

entrada → ⬛️  → salida

Digamos que la caja negra, es computer science, aquella magia que está entre los input y los outputs. Ahora bien, hay diferentes modos de representar la información que se proporciona como entrada o salida de un proceso, ejemplo, podemos contar a los alumnos que hay en una clase con los dedos de las manos y al terminar los diez dedos que tenemos iniciamos otra vez, acarreando un dígito para ir acumulando un conteo mayor, de aquí surge el sistema decimal que usamos:

> 0 1 2 3 4 5 6 7 8 9

Es un sistema de numeración que usamos comúnmente los humanos para numerar todo aquello que nuestra mente puede procesar. En el caso de las computadoras, usan el sistema binario, que consiste solo en dos dígitos:

> 0, 1

Las computadoras están construidas por circuitos en los que realmente solo hace sentido encendido/apagado, pues es la única forma en la que pueden recibir información: electricidad fluyendo por sus cables, entonces solo pueden estar con energía o sin ella, encendido o apagado, 1 o 0. Luego estos estados se convierten en su unidad básica de información, mejor conocida como *bit*.

La pregunta es ¿cómo podría una compuadora contar más allá del 1? Si utilizamos más de un cable para contar, podríamos replicar el modelo de acarreo mental que hacemos con los dedos de nuestra mano o el sistema decimal, estamos muy acostumbrados a abstraer por ejemplo, el número 123, pero si lo analizamos con detenimiento, nos damos cuenta de que 123 = 100 + 20 + 3, cada transición de las unidades a las decenas, implica que hay diez unidades, lo mismo para las centenas con las decenas y así sucesivamente.

Supongamos que tenemos tres focos, el 1 representa encendio, el 0 apagado, podríamos usar la siguiente lógica para contar más que solo 1:

|Estado de los focos|Número decimal|
|-|-|
|0  0  0|0|
|0  0  1|1|
|0  1  0|2|
|0  1  1|3|
|1  0  0|4|
|1  0  1|5|
|1  1  0|6|
|1  1  1|7|

Podríamos contar hasta el 7 con tres focos usando el mismo patrón de acarreo que usamos con los números decimales, realmente, estamos contando 8 elementos.

Las computadoras, emplean este sistema para representar muchos datos y los intercambian gracias a miles y miles de cables controlados por dispositivos muy diminutos llamados *transistores*.

Si regresamos al ejemplo del sistema decimal, y representamos un número en columnas, nos damos cuenta de que cada posición de los dígitos, representa el número 10 elevado a la potencia de la posición en la que se encuentran:

|10^2|10^1|10^0|
|-|-|-|
|#|#|#|

En el sistema binario sucede exactamente lo mismo pero con base 2:
|2^2|2^1|2^0|
|-|-|-|
|#|#|#|

Visto de otro modo, podemos generar 8 permutaciones con los valores 0 y 1 entre columnas, el resultado en decimal sería sumar horizontalmente cada 1 en su correspondiente poderación:

|4|2|1|Suma de unos ponderados|Resultado en decimal|
|-|-|-|-|-|
|0|0|0|0 + 0 + 0|0|
|0|0|1|0 + 0 + 1|1|
|0|1|0|0 + 2 + 0|2|
|0|1|1|0 + 2 + 1|3|
|1|0|0|4 + 0 + 0|4|
|1|0|1|4 + 0 + 1|5|
|1|1|0|4 + 2 + 0|6|
|1|1|1|4 + 2 + 1|7|

Este modelo lo podemos escalar a cualquier número de elementos, ejemplo, cómo saber qué número equivale en decimal al 110101:

|32|16|8|4|2|1|
|-|-|-|-|-|-|
|1|1|0|1|0|1|

Entonces sabríamos que es el resultado de sumar 32 + 16 + 8 + 2 = 58

Ahora nos ataca otra pregunta, ¿cómo representamos letras? la respuesta es: a cada letra se le asigna un número que la representa, para esto se creó un estándar llamado ASCII, que asigna a cada letra un valor empezando por el 65:

|...|A|B|C|D|E|F|G|...|
|-|-|-|-|-|-|-|-|-|
|...|65|66|67|68|69|70|71|...|

Esto significa que si recibimos un mensaje que contiene los símbolos de los números 72, 73, recibiríamos las letras HI. Podríamos ver las equivalencias de ASCII en este [enlace](https://asciichart.com/)

Como podemos ver, la mayoría de los números usados en ASCII se pueden formar con 8 bits, por lo cuál se emplearon por muchos años unidades de 8 bits para enviar estos paquetes de información (letras). El conjunto de 8 bits, se denomina *byte*.

Pero ASCII es un sistema americano, que además está limitado a las permutaciones de un byte, es decir: 256 posibles símbolos, en los cuáles sería imposible representar caractéres con acentos, símbolos asiáticoso arábicos, figuras, etc. Es por ello que eventualmente se empezaron a usar 16 o hasta 32 bits. Hoy en día se utiliza un estándar universal llamado Unicode, que no solo soporta el Inglés, sino trata de representar todos los caractérese de todos los idiomas e incluso un alfabeto muy famoso que se usa hoy en día, los emojis, en el cuál, la carita riendo 😂  es algo así como 1001100010000010.

Ahora nos preguntamos ¿cómo representan colores las computadoras? en este caso, partimos de que una gran cantidad de colores pueden ser representados como combinaciones de tres colores elementales: Rojo, Verde y Azul, entonces, cada pixel de nuestra pantalla, es una mezcla de esos colores en diferentes niveles de intensidad, en este caso la intensidad puede ir desde 0 hasta 255:

|R|G|B|
|-||-|
|255|255|255|

Entonces los colores en un pixel no son más que una interpretación de la intensidad de las cobinaciones de estos tres colores. Consideremos que nuestras pantallas, están conformadas por miles y miles de pixeles y las imágenes, no son otra cosa que matrices de estos pixeles interpretados cada uno por niveles de RGB.

Ahora bien ¿cómo se representan los videos? cambiando la información de cada uno de los pixeles de manera rápida.

Podemos decir en general, que todos los archivos, están clasificados de acuerdo a su extensión, que no es otra cosa que un patrón para ordenar unos y ceros.

# Algoritmos
Son instrucciones paso a paso, a modo de una receta, que programamos en las computadoras para que sean ejecutados. A diferencia de nosotros, las computadoras requieren recibir instrucciones muy precisas de todo lo que tienen que hacer, pues dichas instrucciones serán traducidas al lenguaje que conoce la máquina, esto es: unos y ceros.

Tomemos el ejemplo de un libro de teléfonos (sección amarilla). Si quisiéramos buscar un nombre en ese enorme libro, una opción sería pasar hoja por hoja buscando el teléfono que queremos, lo cuál quizá nos tomaría mucho tiempo, otra opción sería pasar de dos hojas en dos hojas, lo cuál tomaría la mitad de tiempo pero reduciría nuestras probabilidades de encontrar el teléfono un 50%. En cambio, podemos usar otra estrategia: pararnos a la mitad de todo el libro y mirar si nuestro contacto buscado puede estar a la derecha o a la izquierda, si nos decidimos por un lado, hemos encontrado una manera de reducir mucho nuestra búsqueda, si con esa mitad repetimos la operación, hemos vuelto a reducir y si lo hacemos indefinidamente hasta estar en la hoja donde se ubica el contacto buscado habremos eficientado exponencialmente la velocidad de nuestra búsqueda: esto es un algoritmo conocido como *divide y vencerás*.

¿Cómo traducimos esto a código? Sin importar qué lenguaje vayamos a utilizar para traducir esto a código, es importante primero expresarlo en *pseudo código*, que es una aproximación de las instrucciones o pasos que luego programaremos en nuestro idioma natural, el algoritmo anterior podría representarse cómo:
1. **Tomar** el libro
2. **Abrir** el libro a la mitad
3. **Ver** la página
4. *Si* "la persona está en la página"
  * **Llamarle**
5. *Si* "la persona no está en la página" y está a la izquierda
  * **Abrir** la mitad izquierda a la mitad
  * -Ir al paso 3-
6. *Si* "la persona no está en la página" y está a la derecha
  * **Abrir** la mitad derecha a la mitad
  * -Ir al paso 3-
7. *Si* no está en ninguna página
  * **Salir**

Notemos que, las palabras en **negritas** son verbos o acciones, mismas que en el código se llamarán *funciones*, asimismo; las palabras en *cursiva* que representan decisiones, las conoceremos como *condicionales*; las expresiones "entre comillas" son las sentencias que dirigirán nuestras condiciones, las conoceremos como *expresiones booleanas*; las expresiones -entre giones- son saltos en nuestro algoritmo, que conoceremos como *loops*.

Para empezar a indicarle esto a la computadora, podemos utilizar un lenguaje llamado C, en el que inicialmente podemos usar la siguiente notación:
```c
#include <stdio.h>
int main()
{
  printf("Hello, world\n");
}
```

Pero dado que hay mucha sintáxis que veremos más adelante, representaremos nuestros algoortimos en una interfaz más amigable llamada **Scratch** para enfocarnos en la lógica sin preocuparnos por la sintáxis de un lenguaje. Ésta es una herramienta creada en el MIT justo para este fin, en ella podemos generar rutinas que sean obedecidas por objetos mediante *grag & drop* de bloques que ejecutan ciertas acciones.