v0.1.0 🐦 

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

# Vistas y controles
Las vistas, son aquellos elementos que el usuario puede ver, ejemplos:
* Textos
* Imágenes
* Divisores
* Contenedores
Los controles en cambio, aunque se pueden ver, el usuario también puede interactuar con ellos, ejemplos:
* Botones
* Menús
* Cajas de texto

## Creando una vista
1. Creamos en nuestro proyecto un nuevo archivo de tipo SwiftUI
2. Le asignamos un nombre, en este caso será `TextMod`
    Si queremos que se muestre en nuestro entrypoint, habrá que implementar una instancia en nuestro archivo App:
    ```swift
    import SwiftUI

    @main
    struct PlatziApp: App {
        var body: some Scene {
            WindowGroup {
                TextMod()
            }
        }
    }
    ```
    Por defecto nos genera este hola mundo.
3. Podríamos modificar el elemento para mostrar otro texto a nuestro gusto:
    ```swift
    import SwiftUI

    struct TextoMod: View {
        var body: some View {
            Text("Hello, World")
        }
    }

    struct TextoMod_Previews: PreviewProvider {
        static var previews: some View {
            TextoModificadores()
        }
    }
    ```
    Otro aspecto a observar es que, nuestra vista se delimita con un borde azul para que la identifiquemos.
4. Para agregar modificadores o estilos, podemos abrir la columna del lado derecho en las properties y cambiar el tamaño, el tipo de letra, el color, la alineación y el modelo de caja, irá agregando este tipo de props en el código:
    ```swift
    struct TextoMod: View {
        var body: some View {
            Text("Hello, World!")
                .font(.title)
                .foregroundColor(Color.blue)
                .padding(.vertical, 10.0)
        }
    }
    ```
5. Un modificador importante es `frame`, que nos permite definir un contenedor de un tamaño determinado y una alineación:
    ```swift
    Text("Hello, World!")
        .font(.title)
        .foregroundColor(Color.blue)
        .padding(.vertical, 10.0)
        .frame(width: 300, height: 100, alignment: .leading)
    ```
    Nótese que importa el orden en el que se van añadiendo las props, de preferencia añadir primero las del item y luego las del frame si es que lo hemos definido.

## Botones
Empecemos analizando la prop VStack, que nos permite generar una pila de elementos para que éstos sean devueltos en una sola vista:
```swift
struct Buttons: View {
    var body: some View {
        VStack{
            Text("Hello, World!")
            Text("Hello, World!")
        }
    }
}

struct Buttons_Previews: PreviewProvider {
    static var previews: some View {
        Buttons()
    }
}
```
Consideremos que hay un límite de argumentos que se puede usar dentro de un stack. Lo que podemos hacer es anidar stacks.
Bien, para generar un botón dentro de nuestro stack, usaremos la clase `Button` con los argumentos `title:` y `action:`:
```swift
VStack{
    Button("Mi primer boton", action: {
        // código que se quiere ejecutar
        print("Pulse mi boton")
    })
}
```
Los argumentos que hemos introducido, son respectivamente el label del botón y la función que ejecutará, misma que no requiere de especificar nombre y la forma en la que se declara hace que se le denomine *closure*.
Cuando usamos un closure como último argumento, se escribir de esta forma:
```swift
Button("Mi segundo boton") {
    print("Mi segundo boton")
}
```
Una última forma de hacerlo y la que nos permite hacer botones con otro tipo de vistas que no sean un texto y con una función definida exteriormente es la siguiente:
```swift
var body: some View {
    VStack {
        Button(action: {
            saludo()
        }, label: {
            Text("Boton con label como argumento")
        })    
    }

    func saludo() {
        print("Hola desde un metodo definido de manera externa")
    }
}
```
> Si solo tenemos una línea en el argumento action, se pueden eliminar las llaves y dejar solo el nombre del método.

## Imágenes
1. Primeramente, para importar una imagen al proyecto, sleccionamos nuestra carpeta Assets.xcassets.
2. Damos click al botón + que se encuenta en la primera columna en la barra de abajo.
3. Seleccionamos la opción import y buscamos nuestra imagen.
4. Si queremos agregar imágenes de mayor resolución para dispositivos de mayor tamaño, es necesario nombrarlos con sufijo @2x o @3x.
5. Agregamos la imagen a nuestra pila de la siguiente manera:
    ```swift
    var body: some View {
        VStack {
            Image("image_name")
        }
    }
    ```

Algunos modificadores para las imágenes que podríamos agregar son:
```swift
Image("uzi")
    .resizable() // hace que se ajuste el tamaño
    .aspectRatio(contentMode: .fit) // mantiene el aspect ratio
    .frame(width: 100, height: 100, alignment: .center) // cambia el tamaño y la posicion
```

Otros modificadores:
* Blur: `.blur(radius: 3.0)`
* Opacidad: `.opacity(0.8)`

Para hacer una imagen botón:
```swift
Button(action: {}, label: {
    Image("uzi")
        .resizable()
        .aspectRatio(contentMode: .fit)
        .frame(width: 100, height: 100, alignment: .center)
})
```

Para implementar íconos del sistema iOS, podemos basarnos en la app [SF Symbols](https://developer.apple.com/sf-symbols/) y encontrar el nombre del ícono que necesitemos, la forma de implementarlos es la siguiente:
```swift
Image(systemName: "moon.fill")
```

## Input fields
Crearemos un stack nuevo y una variable fuera del mismo, la cuál nos permitirá almacenar el texto
que el usuario genere dentro del campo de texto:
```swift
struct TextFields: View {
    var textoVista:String = "Hola"

    var body: some View {
        VStack {
            Text(textoVista)
        }
    }
}
```

Ahora generaremos un botón que nos permita modificar dinámicamente el valor de la variable que
previamente habíamos dado un valor por default:
```swift
struct TextFields: View {
    // Property wrapper
    @State var textoVista:String = "Hola"

    var body: some View {
        VStack {
            Text(textoVista)

            Button(action: {
                textoVista = "Uzi"
            }, label: {
                Text("Cambia el texto de la vista")
            })
        }
    }
}
```

Nótese que hemos agregado un *property wrapper* a nuestra variable denotado por `@State` este
wrapper tiene la función de generar un **state** de la aplicación, esto quiere decir, que va
envolver la property para que el valor de la misma se almacene fuera de la estructura aunque las
vistas se destruyan (ya que siempre que hay algún cambio se destruyen y se reconstruyen las vistas);
de esta forma, aunque nuestro stack se re-calcule, los valores almacenados en el **state**, prevalecerán.

Ahora generaremos nuestro field:
```swift
struct TextFields: View {
    @State var textoVista:String = "Hola"
    
    var body: some View {
        
        VStack {
            Text(textoVista)
            
            TextField("Escribe algo", text: $textoVista)
            
            Button(action: {
                textoVista = "Uzi"
            }, label: {
                Text("Cambia el texto de la vista")
            })
        }
    }
}
```

Notamos que el `TextField` recibe dos parámetros en este caso: un título (placeholder) y un *text binding*,
que traducido al español podría entenderse como *texto amarrado* ya que cada que se modifique el
valor del input, los cambios se propagarán a la variable, para ello se coloca el signo de `$`.

## Dividers
Para generar formas dentro de la pantalla podemos utilizar la siguiente estructura:
```swift
var body: some View {
    VStack {
        Circle()
    }
}
```

Hasta este punto no le hemos determinado dimensiones o un frame, para hacerlo, le agregamos el modificador
`frame`, con el cuál, limitaremos la figura al frame que determinemos:
```swift
    Circle().frame(width: 100, height: 100, alignment: .center)
```

Para generar un divisor, utilizaremos la estructura `Divider`, siempre procurando configurarle un frame para
que los elementos tengan distancia de separación:
```swift
    Divider().frame(height: 16)
```

# Contenedores

## HStack, VStack y spacer
Ya hemos revisado el `Vstack` como pila de elementos, ahora identificaremos el `HStack`, si bien el primero
nos permitía introducir varios elementos en una vista de manera vertical, el segundo nos permitirá hacerlo
de manera horizontal:
```swift
    VStack(alignment: .trailing) {
        Text("Hello, World!")
        Text("Placeholder")
        Text("Placeholder")
            
        HStack {
            Text("Mexico")
            Text("Mexico")
            Text("Mexico")
        }
    }.border(Color.black)
```


> 💡 Para agregar elementos, podemos pulsar el botón + en la parte superior derecha

> 💡 Para embeber un item dentro de un stack, usamos opt + cmd + click en el elemento a embeber,
posteriomente embed in VStack o HStack

> 💡 Podemos agregar modificadores al final de una estructura cuando sea de tipo vista

Nótese que estamos introduciendo el argumento `alignment` al `VStack` con la finalidad de alinear de cierto
modo su contenido.

La clase `spacer` nos ayuda a generar un espacio entre nuestras diferentes vistas de manera que se llenen
los espacios que estén vacíos, esto dependiendo del tipo de stack en el que estemos, en el siguiente ejemplo,
para un `Vstack` nos generaría espacios entre los tres textos para llenar toda la pantalla:
```swift
VStack(alignment: .trailing) {
    Text("1").border(Color.black)
    Spacer()
    Text("2").border(Color.black)
    Spacer()
    Text("3").border(Color.black)
}
```

## ZStack
Este stack nos permite acomodar vistas en el eje de la Z, es decir, de atrás hacia adelante.
```swift
var body: some View {
    ZStack {
        Color.yellow
        Text("Uzi")
    }.ignoresSafeArea()
}
```

> 💡 El modificador `ignoreSafeArea()` nos ayuda para que cuando queramos rellenar una vista de color por ejemplo,
se pinten aún la zona del botón home y el notch

Para emplear el `Stack` hagamos el siguiente ejercicio: pongamos un input field, y modifiquemos
el color del placeholder (ya que actualmente no hay un modificador para esta parte del input):
```swift
var body: some View {
    ZStack {
        Color.yellow
        VStack {
            Image("uzi")
                .resizable()
                .aspectRatio(contentMode: .fit)
                .frame(width: 100, height: 100, alignment: .center)
            Text("Bienvenidos")
                
            ZStack {
                if curso == "" {
                    Text("Curso").foregroundColor(.blue)
                }
                TextField("", text: $curso).padding(.leading, 16.0)
            }
        }
    }.ignoresSafeArea()
}
```
Observemos cómo se ha puesto el placeholder artificial detrás del input controlado por un if que
nos permita esconderlo cuando el usuario ya haya escrito algún valor.

## Dividir nuestra aplicación en módulos de contenedores
Cuando nuestro código empieza a crecer, surge la necesidad de ir modularizando las partes de la UI
para hacerlo más legible y sostenible. Para ello, podemos crear otra estructura que retorne un tipo
`View`, ejemplo:
```swift
import SwiftUI

struct ZStackPadding: View {
    // Código de la vista principal
    Imagenes()
    // Código de la vista principal
}

struct Imagenes: View {
    var body: some View {
        Image("uzi")
            .resizable()
            .aspectRatio(contentMode: .fit)
            .frame(width: 100, height: 100, alignment: .center)
    }
}

struct ZStackPadding_Previews: PreviewProvider {
    static var previews: some View {
        ZStackPadding()
    }
}
```

Otra manera sería agregar la instancia de nuestra estructura `Imagenes` dentro del `Previews`:
```swift
struct ZStackPadding_Previews: PreviewProvider {
    static var previews: some View {
        ZStackPadding()
        Imagenes()
    }
}
```

El Previews nos puede servir para ir observando una vista en particular y finalmente agregarla cuando
esté lista a la vista principal. De esta forma podemos generar todos los views que necesitemos con el
fin de modularizar el código y generar piezas reutilizables. Esta separación e instanciación de código,
también se puede hacer de la misma forma entre archivos.

# TabView
Es otro tipo de vista que nos permitirá generar varias vistas dentro de una ruta de nuestra app.
Una vista de tipo `TabView` también puede agrupar a otras vistas al igual que los stacks, pero en
este caso, nos servirán para generar un switch de vistas dentro de una ruta de la app:
```swift
struct Tabviews: View {
    var body: some View {
        TabView {
            TextoModificadores().tabItem {
                Text("Pantalla 1")
                Image(systemName: "moon")
            }
            
            TextFields().tabItem {
                Text("Pantalla 2")
                Image(systemName: "play")
            }
            
            ZStackPadding().tabItem {
                Text("Pantalla 3")
                Image(systemName: "terminal")
            }
        }
    }
}
```
Nótese que cada vista que se inserta dentro de `tabView`, luego es modificado mediante `tabItem`,
y éste último a su vez, nos requiere dos argumentos: `Text` e `Image`, que no son sino el título
y el ícono de la tab que se formarán en el bottom de la aplicación para hacer esta navegación.

# Navigations
## NavigationView
Para generar más rutas en nuestra aplicación, utlizaremos el `NavigationView`, éste se configura de
la siguiente manera:
```swift
var body: some View {
    NavigationView {
        Text("Hello, World!")
            .navigationTitle("Home")
    }
}
```
De esta forma ya le hemos dado a la vista `Text` el carácter de una ruta de la aplicación, cuyo
*header* es *Home*.

Veamos algunos modificadores, como:
* `navigationBarTitleDisplayMode` que nos permite cambiar el tipo de ruta en la que estamos y
modifica el header para casos de pantallas quizá con menor jerarquía.
* `toolbar` que nos da la opción de agregar elementos en el header de la aplicación.
```swift
    Text("Hello, World!")
        .navigationTitle("Home")
        .navigationBarTitleDisplayMode(.inline)
        .toolbar(content: {
            ToolbarItem(placement: .navigationBarTrailing, content: {
                Button(action: {}, label: {
                    Text("+")
                })
            })
        })
```

## NavigationLink
Ahora veamos cómo agregar un link que nos permita generar la navegación hacia otra ruta, ésta
se hace mediante el `NavigationLink`, para ello podemos poner nuestra vista anterior en un stack
junto con este elemento, el cuál requerirá como argumentos un label y un destination, es decir
la vista destino.
```swift
var body: some View {
    NavigationView {
        VStack {
            Text("Hello, World!")
                .navigationTitle("Home")
                .navigationBarTitleDisplayMode(.inline)
                .toolbar(content: {
                    ToolbarItem(placement: .navigationBarTrailing, content: {
                        Button(action: {}, label: {
                            Text("+")
                        })
                    })
                })
            NavigationLink("Navegar a vista de texto", destination: Text("Hola desde otra vista"))
        }
    }
}
```
Nótese que la ruta hacia la que estamos navegando es muy simple, pero en la práctica, se colocará
normalmente otra vista más compleja que hayamos generado previamente, ejemplo, la vista `Tabviews`
que generamos en otras lecciones.

### Variantes del NavigationLink
Si requerimos generar nuestra navegación mediante otro elemento que no sea un texto, o trasladar
esta acción a otro elemento, podemos implementar lo siguiente:
```swift
struct Navigations: View {
    // State de la vista
    @State var isDividersActive: Bool = false;
    
    var body: some View {
        NavigationView {
            VStack {
                Text("Hello, World!")
                    .navigationTitle("Home")
                    .navigationBarTitleDisplayMode(.inline)
                    .toolbar(content: {
                        ToolbarItem(placement: .navigationBarTrailing, content: {
                            Button(action: {
                                isDividersActive = true // Habilitar la ruta divider
                            }, label: {
                                Text("+")
                            })
                        })
                    })
                NavigationLink(
                    destination: Dividers(),
                    isActive: $isDividersActive,
                    label: {
                        EmptyView() // Vista vacía
                    })
            }
        }
    }
}
```
Nótese que ahora estamos usando otros argumentos, como `isActive`, que no es otra cosa sino un flag
para indicar que está activa otra vista (la indicada en `destination`). Asimismo `label`, que es
donde se colocará la vista que controlará nuestro routing.

# Práctica
Practiquemos un poco todo el contenido generando la siguiente vista en la que usaremos una imagen como
botón de inicio para un botón de player que nos direccione hacia otra vista con un video player:
```swift
import SwiftUI
// Libreria para video player
import AVKit

struct Player: View {
    @State var isPlayerActive = false
    
    var body: some View {
        NavigationView {
            VStack {
                Button(action: {
                    isPlayerActive = true
                }, label: {
                    ZStack {
                        Image("uzi")
                            .resizable()
                            .aspectRatio(contentMode: .fit)
                        // Icono de player sobrepuesto
                        Image(systemName: "play.fill")
                            .foregroundColor(.white)
                    }
                })
                
                NavigationLink(
                    destination: VideoPlayer(
                        player: AVPlayer(
                            url: URL(string: "https://cdn.cloudflare.steamstatic.com/steam/apps/256705156/movie480.mp4")!
                        )
                    ).frame(width: 420, height: 360, alignment: .center)
                    ,
                    isActive: $isPlayerActive,
                    label: {
                        EmptyView()
                    })
            }
        }
    }
}
```