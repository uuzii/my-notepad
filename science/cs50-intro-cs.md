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