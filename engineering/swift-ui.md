# Qué es SwiftUI
Es un framework para crear interfaces de usuario para todas las plataformas de apple, reutilizando elementos ya desarrollados. Para emplearlo es necesario concer Switft y tener instalado Xcode.

# Creación de una app
Para crear una app en Xcode, seleccionaremos Crear un nuevo proyecto en la pestaña de iOS, luego en la ventana siguiente pondremos un nombre a la app en Product name, luego organization name, precedido por "com", con lo cuál se generará el bundle identifier.
En Interface, seleccionaremos SwiftUI, luego en lifecycle: SwiftUI App. Core data no la seleccionaremos, pero su función es ser una especie de base de datos, Test es para hacer unit testing, de momento no nos enfocaremos en ellas.

## Xcode
* Navegador, es la barra lateral izquierda donde navagaremos a través de todos los archivos del proyecto.
* Editor, es la parte central, donde editaremos nuestro código.
* Inspector, la barra lateral derecha, es el inspector, que es donde veremos las opciones que tienen las partes de código que estemos modificando.
* Simulador, nos permite emular todo un hardware.

# Bases de SwiftUI

## Variables y constantes
Para practicar alguos conceptos, usaremos un Playground, pero en lugar de importar `UIKit`, usaremos `Foundation`:
```swift
var str = "Hello, playground"
print(str)
```

Con estas líneas, imprimiremos en consola un mensaje: `var` es para la creación de una variable seguido por su nombre y finalmente se le asigna su valor.

Para generar constantes, usamos la palabra reservada `let`: 
```swift
let name = "Uzi Rodriguez"
```

Swift hace una coersión natural para definir el tipo de dato con el que queremos trabajar, pero también podemos indicar tipos de datos usando `:`:
```swift
let x:Int = 2
let y:Int = 2
print(x+y)
```

## Funciones y argument labels
Una de las funciones más usadas es `print`, que ya la hemos usado en los ejemplos anteriores.

Para crear una función con argumentos, esta es la estructura básica:
```swift
func suma(x:Int, y:Int) {
    print("Parámetros:", x, y)
}

suma(x: 1, y: 2)
```

Si queremos que la función retorne un tipo de dato, usamos una flecha:
```swift
func suma(x:Int, y:Int) -> Int {
    return x + y
}

var resultado = suma(x: 1, y: 2)
print("El resultado fue \(resultado)")
```

Podemos generar un alias a los argumentos dentro de la función de la siguiente manera:
```swift
func saluda(a destinatario:String, de remitente:String) {
    print("Mando saludo a \(destinatario) de parte de \(remitente)")
}

saluda(a: "Juan", de: "Uzi")
``` 

Si no queremos tener que especificar el nombre del argumento, podemos poner un `_`, pero esto hará que el argumento sea pediso siempre que se invoca la función:
```swift
func saluda(_ destinatario:String, de remitente:String) {
    print("Mando saludo a \(destinatario) de parte de \(remitente)")
}

saluda("Juan", de: "Uzi")
``` 



