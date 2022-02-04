# Expresiones regulares
Las expresiones regulares tienen mucha potencia para hacer operaciones muy pesadas, hacer una inversión en ellas es como adquirir una *navaja suiza* para el tratamiento de datos en general.

Las expresiones regulares pueden ser muy complejas, pero en el fondo, no son otra cosa que patrones, que buscaremos en archivos de texto.

Podemos hacer búsquedas en textos enormes mediante parámetros o patrones de:
* Número de dígitos
* Caractéres especiales
* Caractéres ASCII o no ASCII
* Entre otros

Éstas pueden ser excluyentes entre sí o no necesariamente excluyentes para clasificar datos que queremos usar o no usar en un programa

> 💎  Las expresiones Regulares, están atadas a prácticamente cualquier lenguaje y son mucho más rápidas que la lógica propia del lenguaje y nos permiten extraer datos para hacer operaciones con ellos directamente.

Las expresiones regulares se diferencían de un Ctrl + F en que nos permiten buscar patrones en un texto, no solo una secuencia de caractéres. Algunas aplicaciones prácticas que se les dan son:
* Validar correos en una aplicación
* Validar calidad de los datos en un input
* Encontrar patrones dentro de un log arrojado por un servidor

# Lenguaje de las expresiones regulares
Si quisiéramos encontrar un teléfono en México (de 10 dígitos), veremos que los datos pueden estar presentados de diferentes modos: pueden estar solo los diez dígitos, los dígitos separados de dos en dos, los dígitos separados por guiones. Una forma de hacer esto, sería más o menos la siguiente:
1. Expresarle a un *motor de expresiones regulares*: Encuentra todas las apariciones de diez dígitos que pueden estar separados por [ ] [,] [-]
2. Vamos a abstraer lo que buscamos (dígitos) a clases, un dígito se puede expresar como `-d`, una palabra como `-w`
3. Una vez buscada la forma, buscaremos repetición, ejemplo, si queremos que sean de 8 a 10 (incluso se puede validar de 1 a n, de 0 a n, todas las combinaciones que se nos ocurran)

## caracter (.)
El primer elemento que veremos es el caracter, que es la unidad mínima de un archivo de texto, y se busca mediante un punto `.`

* Si quisiéramos buscar un caracter que despues tenga un espacio, usaríamos: `. `
* Si quisiéramos buscar diez caractéres juntos, usaríamos: `..........` (que realmente es la ocurrencia de 10 caractéres juntos)

Del caracter se subdividen tres clases principales:
* El dígito `\d`
  * `\d\d\d`: buscaría las ocurrencias de tres dígitos contiguos
  * Una equivalencia de `\d` es `[0-9]`
  * Para buscar dígitos de manera más potente, podríamos buscar `[0-5]`
* La palabra `\w` incluye conjuntos de caractéres con dígitos, letras de la A-Z mayúsculas y minúsculas y excluye caractéres especiales
  * Para hacer búsquedas más potentes, podemos usar `[a-f]`
* Los espacios (incluyen tabs y en algunos casos el salto de línea) `\s`

Para escapar caractéres como el punto per se, podemos usar `\.`
Podríamos buscar solo letras específicas con `[ABC]`

Vemos entonces, que existirían las siguientes equivalencias:
* `[0-9]` ~ `\d`
* `[0-9a-zA-Z_]` ~ `\w`

## Delimitadores (+. *, ?)
* Cero o n `*` encontraría *todos* los elementos indicados, ejemplo: `.*`
* Uno o más `+` encontraría *uno o más*, ejemplo `\d+[a-z]` buscaría que haya uno o más dígitos y al final una palabra
* Cero o uno `?` encuentra donde haya *uno o ningún elemento*, ejemplo: `\d*[a-z][a-z]?` buscaría: que haya dígitos, luego una letra y luego que haya o no haya una letra
* Si solo ponemos un caracter sin estos símbolos, tiene que estar dicho caracter obligatoriamente

## Contadores {x,y}
Nos permitirán buscar un determinado número de apariciones de algún patrón, una equivalencia de `\d\d\d` sería `\d{3,3}`, donde el primer argumento es la cota inferior, y el segundo argumento es la cota superior, éstas bien podemos omitirlas:
* `\d{4,}`: pediría al menos cuatro dígitos y no habría límite

## (?) Como delimitador
Si quisiéramos reducir el tamaño de un match, podemos usar el signo ?, ejemplo, para el siguiente archivo:
```csv
csv1,csv2,csv3,csv4,csv5,
```

Si usáramos, la expresión `.+,`, encontraríamos toda la línea, pero si quisiéramos reducir nuestra búsqueda a cada match de los elementos separados por comas, usaremos: `.+?,`

## Not (^)
Para las clases más generales, ejemplo `\d`si la colocamos en mayúcula `\D` encontraremos lo que no sea un dígito. Pero hay una forma de hacer esto para las clases custom que hagamos, ejemplo, `[^0-5]` nos encontraría lo que no haga match con este patrón.

Otro ejemplo, sería para validar los siguientes teléfonos:
```csv
555555
55-55-55
55.55.55
```

Podríamos usar la expresión `\d\d\D?\d\d\D?\d\d`









