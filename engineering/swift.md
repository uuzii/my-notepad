# Swift
Swift es un lenguaje creado por Apple para sustituir el uso de Objetive C. Para desarrollar Apps de iOs hoy es indispensable conocer Swift. Mantiene algunas semejanzas con C, pero agrega features como:
* Tipos opcionales
* Tuplas
* Variantes en POO
* Autocompletado muy completo
* Documentación

Primeramente, para empezar a usar este lenguaje, es recomendable utilizar Xcode. Es importante también que conozcamos y tengamos a la mano el sitio de [Developer](https://developer.apple.com/).

## Hello world

Al iniciar Xcode veremos que nos presenta varias opciones, en este caso utilizaremos un [Playground] que es un archivo que nos permitirá testear el lenguaje.

Para generar una variable e imprimirla, empezamos escribiendo:
```swift
import UIKit

var str = "Hello, playground"
print(str)
```

La mayor parte de los datos son comunes a otros lenguajes, pero Swift tiene particularidades como las *tipos de datos opcionales*. También tomemos en cuenta que Swift es un *type safe language*, lo cuál quiere decir que cuando creamos una variable con cierto valor, infiere qué tipo de dato es, no hace falta especificarlo.

## Variables, constantes, comentarios

En swift existen tanto variables como constantes. Para las constantes usamos la palabra reservada `let` y para variables `var`:
```swift
// constante
let maxnumberOfLoginAttemps = 3
// variable
var currentLoginAttemp = 0
```

Se pueden declarar múltiples variables en la misma línea separándolas por coma:
```swift
var x = 0.0, y = 0.0, z = 0.0
```

## Type annotations
Si no indicamos un valor inicial a una variable, tenemos que indicar cuál es le tipo de dato que puede tomar, ejemplo:

```swift
var welcomeMessage : String
```

Si quisiéramos generar varias variables de este modo, podríamos usar:
```swift
var red, green, blue : Double
```

A esta práctica se le denomina *type annotation*. Es importante usarlas con el fin de evitar errores de compilación.

## Nomenclatura
Usualmente, nombraremos todas las variables en inglés, una de las técnicas que usamos comúnmente es *camel case*, lo cuál muchas veces complica la lectura pero es la más estandarizada. Otra técnica es el *snake case* pero no es la más estandarizada.
```swift
var camelCase = "variable"
var snake_case = "variable"
```

Swift, nos permite nombrar variables con más caractéres que solo letras, por ejemplo símbolos, caractéres de otros alfabetos, emojis, etc (lo cuál no es muy buena práctica) Ejemplo:
```swift
var 🔥 = "fire"
```

## Interpolación de strings
Para generar strings enriquecidos con variables o constantes, podemos hacer una interpolación de la siguiente manera:
```swift
var number = 0
var total = 3
print("El número actual es: \(number) de un total de \(total)")
```

## Comentarios
Para escribir comentarios (texto de ayuda que no se ejecuta en nuestro código), podemos usar las siguientes formas:
```swift
// comentario de una línea
/**
  comentario
  multilinea

  /**
    comentario multilinea
    anidado
   */

 */
```

Cabe señalar que también swift nos permitirá anidar comentarios, esto s eutilizará cuando queramos desactivar ciertas partes del código con todo y sus comentarios sin ningún problema.

# Tipos de datos
Algunos de los tipos de datos que existen en swift son:

## Enteros
Éstos son todos los números que no contienen decimal (incluyendo a los negativos y al cero):
```swift
var age = 31
```

Adicional, swift nos proporciona algunas clases como `UInt8` que nos da acceso a, en este ejemplo, el número entero de 8 bits más pequeño y el más grande:
```swift
let minValue = UInt8.min // 0
let maxValue ) UInt8.max // 255
```

Este tipo de dato, se pueden usar por ejemplo, para variables que no requerirán más de 8 bits, como puede ser el caso de los rangos RGB o la edad de una persona. La finalidad es ahorrar el espacio en memoria que ocuparán nuestras variables.

## Flotantes
Para los números que contienen decimal, utilzaremos `Float` y `Double`, que son de 32 y 64 bits respectivamente para su precisión:
```swift
let f1 : Float = 3.1416

let d1 : Double = 3.14159265
```

Nótese que por defecto, Swift tomará los datos de tipo flotante como Double, por ello es importante identificar si manejaremos pocos decimales de precisión (6 para float, 15 aprox para Double).

Swift nos permite operar con datos que no hemos especificado, ejemplo:
```swift
var anotherPi = 3 + 0.1415 // 3.1415
```

Dado que swift es *type-safe* podrá hacer la suma, pero esto no necesariamente será positivo en todos los casos ya que si las variables no son compatibles para operarlas se pueden generar errores.

## Literales numéricos
Para asignar valores numéricos a variables , podemos usar notaciones en otros sistemas numéricos como son binario, octal y hexadecimal:
```swift
let decimalInteger = 17
let binaryInteger = 0b10001       // 17 en binario
let octalInteger = 0o21           // 17 en octal
let hexadecimalInteger = 0x11     // 17 en hexadecimal
```

Swift también nos permitirá hacer uso de otros tipos como la notación científica, notación exponencial o notación exponencial hexadecimal:
```swift
let decimalDouble = 12.1875
let exponentDouble = 1.21875e1
let hexadecimalDouble = 0xC.3p0
```

También existe la posibilidad de agregar ceros a la izquiera o guiones bajos para hacer más legibles nuestros números:
```swift
let paddedDouble = 000123.456
let oneMillion = 1_000_000
let justOverOneMillion = 1_000_000.000_000_1
```

Tomemos en cuenta que si queremos agregar valores no válidos a los tipos que hemos visto, tendremos errores, ejemplos:
```swift
let cannotBeNegative: UInt8 = -1  // error
let tooBig: Int8 = Int8.max + 1   // error
```


