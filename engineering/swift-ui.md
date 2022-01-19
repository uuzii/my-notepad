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

## Funciones y argument labels
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

Si no queremos tener que especificar el nombre del argumento, podemos poner un `_`, pero esto hará que el argumento sea pedido siempre que se invoca la función:
```swift
func saluda(_ destinatario:String, de remitente:String) {
    print("Mando saludo a \(destinatario) de parte de \(remitente)")
}

saluda("Juan", de: "Uzi")
``` 


## Estructuras
SwiftUi está basado en estructuras, por mencionar algo, todas las vistas son estructuras y por ello es importante conocer su manejo.

### Definición

Esta es la forma elemental de una estrcutra:
```swift
struct calculadora {
}
```

Dentro de ella podemos definir atributos y métodos como en POO para las clases:
```swift
struct calculadora {
    var numeroUno:Int
    var numeroDos:Int
    
    func suma() -> Int {
        var numeroInternoSuma:Int = 11
        return numeroUno + numeroDos
    }
}
```

### Instanciamiento

Para instanciar una estructura, simplemente la invocamos en la declaración de una variable. Para esto es importante también tomar en cuenta que los atributos o propiedades internas se tienen que inicializar o en su defecto, definirlos con un valor por defecto.
```swift
var suma = calculadora(numeroUno: 10, numeroDos: 8)

// Accede a una prop
suma.numeroUno // 10
// Accede  una funcion
suma.suma() // 18
```

> Si el editor nos arroja error, casi siempre tendremos un botón "Fix" que nos autocorregirá

### Scope (alcance)
Es importante notar algunos puntos respecto al ámbito de las funciones que vemos en el ejemplo:
* Los métodos declarados dentro de una estructura no comparten el scope global de la estructura
* Los métodos internos de una estrctura no comparten scope entre sí, sino ue tienen cada uno el propio
```swift
struct calculadora {
    var numeroUno:Int
    var numeroDos:Int
    
    func suma() -> Int {
        var numeroInternoSuma:Int = 11
        return numeroUno + numeroDos
    }
    
    func multiplicacion(numeroUno: Int, numeroDos:Int) -> Int {
        // Si queremos acceder a una variable global y no a la del scope local del método, usamos la palabra reservada self
        return self.numeroUno * numeroDos
    }
}
```


### Diferencias entre estrcuturas y clases
Tomemos en cuenta que las estructuras se pueden inicializar (constructor) a partir del método `init`, mismo en el que se pueden definir valores por defecto dentro de una estructura:
```swift
struct calculadora {
    var numeroUno:Int
    var numeroDos:Int
    
    init() {
        numeroUno = 10
        numeroDos = 20
    }
}
```

> Si no quitamos los argumentos que hemos puesto en el paso de instanciación, el editor nos arrojará error después de haber implementado init, ya que no tiene sentido re asignarles valor

Ahora bien, si impromimos valores tendremos 10 y 20, pero también existe la posibilidad de cambiar los valores de los atributos:
```swift
var suma = calculadora()
suma.numeroUno = 4
suma.numeroDos = 8
suma.numeroUno // 4
suma.numeroDos // 8
```

Si creamos una instancia nueva llamada `instanciaSuma` a partir de la primera instancia, ésta se creará por valor y no almacenará la referencia, lo podemos notar cambiándo valores como la demo de arroba y comprobando que cada una tiene sus valores:
```swift
var suma = calculadora()
suma.numeroUno = 4
suma.numeroDos = 8
suma.numeroUno // 4
suma.numeroDos // 8

var instanciaSuma = suma
instanciaSuma.numeroUno // 4
instanciaSuma.numeroDos // 8
// Mdoficación de la segunda instancia:
instanciaSuma.numeroUno = 20
instanciaSuma.numeroDos = 30
// Nuevos valores segunda instancia:
instanciaSuma.numeroUno // 20
instanciaSuma.numeroDos // 30
// Valores que conserva la primera instancia:
suma.numeroUno // 4
suma.numeroDos // 8
```

Esta es la diferencia entre una estructura y una clase, que la estrcutrura se instancia generando un valor nuevo, la clase lo hará mediante una referencia.

## Computed properties
Si queremos que una variable adopte un valor sin necesidad de tener que ejecutar una función la podemos declarar del siguiente modo:
```swift
var suma:Int {
    10 + 10
}

suma // 20
```
> Las computed props siempre requerirán el tipo de dato

Si tenemos solo una línea de código, no requiere obligatoriamente la palabra reservada `return`, pero si hay más líneas, sí se tendrá que especificar:
```swift
var suma:Int {
    let x = 11;
    return 10 + x
}

suma // 21
```

# Bases de SwiftUI

## Protocolos
Vamos al archivo ContentView.swift, del lado derecho pulsamos el botón Resume y podremos ver un Hola mundo.
Ahora, iremos identificando cada uno de los elementos que vemos en el IDE:
* `PlatziApp.swift`: denota un punto de entrada o pantalla principal, que se compone por una instancia de `ContentView`:
    ```swift
    struct PlatziApp: App {
        var body: some Scene {
            WindowGroup {
                ContentView()
            }
        }
    }
    ```
* `ContentView.swift`: es la definición de nuestra pantalla principal, donde `body` es una computed property, cuyo valor de retorno es un texto con un modificador.
    ```swift
    struct ContentView: View {
        var body: some View {
            Text("Hello, world!")
                .padding()
        }
    }
    ```
    Nótese la parte `: View`, ésta indica que se tiene que seguir un **protocolo**, en este caso, el que sea necesario para mostrar una vista.
    La propiedad interna `body` la estamos configurando con `: some View` porque tiene que devolver alguna vista para cumplir con el protocolo de su estructura padre.

    También contiene otra estructura que utiliza internamente Xcode para mostrar nuestro Canvas del lado derecho:
    ```swift
    struct ContentView_Previews: PreviewProvider {
        static var previews: some View {
            ContentView()
        }
    }
    ```
    Nótese que en esta parte también estamos definiendo un protocolo, mismo que es obdecido por el valor de retorno que es la instancia de nuestro `ContentView`