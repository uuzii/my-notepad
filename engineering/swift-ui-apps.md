v0.1.0 

# Desarrollo de aplicaciones iOS con SwiftUI

## Contenidos
* Implementar arquitectura MVVM en apps creadas con SwiftUI.
* Dividir interfas de usuario en módulos con vistas y controles.
* Recuperar y mostrar información (texto, imágenes y video).
* Implementar repositorios externos para darle funcionalidades no nativas a SwiftUI.

# Intro a MVVM
Modelo - Vista - Vista Modelo. Es un patrón de arquitectura de software que se caracteriza
por tratar de separar lo máximo posible la lógica de la interfaz de usuario.

![MVVM](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/mvvm.jpg?raw=true)

# Planear una aplicación
Antes de empezar a crear nuestra aplicación, siempre es importante planear lo que vamos a construir,
de otro modo los proyectos pueden tener errores de definición. También es importante consultar la 
[documentación](https://platzi.com/clases/2354-swiftui-apps-ios/38986-planeando-nuestra-app/#:~:text=Apple%20Developer%20Documentation,com/documentation/swiftui/)
del lenguaje, el trabajo en equipo, no tratar de memorizar todos los conceptos y apoyarnos en un
[diseño visual](https://platzi.com/clases/2354-swiftui-apps-ios/38986-planeando-nuestra-app/#:~:text=Apple%20Developer%20Documentation,com/documentation/swiftui/).

En la mayoría de los casos, requeriremos también un [API](https://gamestream-api.herokuapp.com/),
de donde obtendremos los datos requeridos para presentar la información al usuario, generalmente serán
archivos de tipo JSON.

También solemos requerir assets o imágenes, que es importante contar con ellas, y por último,
identificar aquellas librerías de terceros que podemos requerir para emplear funciones que no están
inmersas de manera nativa en el lenguaje.

# Creando el proyecto
Para empezar a crear nuestro proyecto, seleccionaremos en Xcode, Crear nuevo proyecto > iOS > App >
Pondremos nombre, ejemplo: `GameStream`. No será necesario seleccionar o mdificar ninguna otra opción.
Posteriormente localizaremos el proyecto en la ubicación que requiramos, preferentemente aceptando
que se cree el repo local.

## Pantalla de inicio
Para la pantalla de login, observamos que es necesario importar primeramente algunos aassets, principalmente
el logotipo:

![swift ui apps login design](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/swift-ui-apps-login.png?raw=true)

Posteriormente, vemos que la aplicación tiene un fondo de color, para ello implementaremos primeramente
un `ZStack`:
```swift
struct ContentView: View {
    var body: some View {
        ZStack {
            Color(red: 19/255, green: 30/255, blue: 53/255, opacity: 1.0)
                .ignoresSafeArea()
        }
    }
}
```

Para la parte superior de nuestra pantalla inicial, insertaremos la imagen y los labels de iniciar sesión,
registrarse en un `VStack:
```swift
ZStack {
    Color(red: 19/255, green: 30/255, blue: 53/255, opacity: 1.0)
        .ignoresSafeArea()

    VStack {
        Image("app_logo")
            .resizable()
            .aspectRatio(contentMode: .fit)
            .frame(width: 250)
            // Labels registro e inicio sesion en vista separada
            InicioYRegistroView()
    }
}
```

Notemos que los labels estarán en una vista separada, que será a su vez, un `HStack`:
```swift
struct InicioYRegistroView: View {
    var body: some View {
        VStack {
            HStack {
                Text("INICIA SESIÓN")
                Text("REGÍSTRATE")
            }
        }
    }
}
```

### Estructura de pantalla de inicio
La pantalla de Inicio, consiste básicamente en el logo y dos botones: "Inicia sesión" y "Regístrate", estos
han de alternar su propia vista, para lo cuál, primeramente generaremos los botones en el `HStack` de la sección
anterior:
```swift
struct InicioYRegistroView: View {
    @State var esInicioDeSesion = true;
    
    var body: some View {
        VStack {
            HStack {

                Spacer()

                Button("INICIA SESIÓN") {
                    esInicioDeSesion = true
                }.foregroundColor(esInicioDeSesion ? .white : .gray)
                
                
                Spacer()

                Button("REGÍSTRATE") {
                    esInicioDeSesion = false
                }.foregroundColor(esInicioDeSesion ? .gray : .white)

                Spacer()
            }
            
            Spacer(minLength: 42)
            
            if esInicioDeSesion == true {
                InicioSesionView()
            } else {
                RegistroView()
            }
        }
    }
}
```
Nótese que hemos generado una variable de `@State` denominada `esInicioDeSesion` con
la finalidad de hacer toggle entre las dos vistas que estarán dispinibles: `InicioSesionView` y
`RegistroView`, además de esto hemos usado la misma variable para togglear el color que tienen
los botones. Por otro lado, hemos añadido algunos detalles de estilos como son los `Spacer`
para generar espacio entre los elementos en pantalla.

Estas serían las definiciones de las vistas secundarias:
```swift
struct InicioSesionView: View {
    var body: some View {
        Text("Soy la vista de inicio de sesion")
    }
}

struct RegistroView: View {
    var body: some View {
        Text("Soy la vista de registro")
    }
}
```

Regresaremos momentáneamente a la parte superior de toda la pantalla para agregar un `Spacer`
encima del logo y para generarle a la imagen un `padding`:
```swift
struct ContentView: View {
    var body: some View {
        ZStack {
            Spacer()
            
            Color(red: 19/255, green: 30/255, blue: 53/255, opacity: 1.0)
                .ignoresSafeArea()
            
            VStack {
                Image("app_logo")
                    .resizable()
                    .aspectRatio(contentMode: .fit)
                    .frame(width: 250)
                    .padding(.bottom, 40)
                
                InicioYRegistroView()
            }
        }
    }
}
```

### Vista Inicio de sesión
#### SecureField y Scroll
Para la vista de inicio de sesión, implementaremos la siguiente estructura:
```swift
struct InicioSesionView: View {
    @State var correo = ""
    @State var contrasena = ""
    
    var body: some View {
        ScrollView {
            VStack(alignment: .leading) {
                Text("Correo electrónico")
                    .foregroundColor(Color("Dark-Cyan"))
                
                ZStack(alignment: .leading) {
                    // text field
                }
                
                Divider().frame(height: 1)
                    .background(Color("Dark-Cyan"))
                    .padding(.bottom)
                
                Text("Contraseña")
                    .foregroundColor(Color("Dark-Cyan"))
                
                ZStack(alignment: .leading) {
                    // Secure field
                }
                
                Divider().frame(height: 1)
                    .background(Color("Dark-Cyan"))
                    .padding(.bottom)
                
            }.padding(.horizontal, 77.0)
        }
    }
}
```

Donde observaremos lo siguiente:
* La vista `ScrollView` se utiliza para que el usuario pueda scrollear, sobre todo para pantallas pequeñas donde no cabe el contenido.
* El `VStack` contendrá todos los elementos del form de manera vertical.
* El `VStack` le definiremos un padding momentáneo para que ocupe un espacio determinado y también un `alignment`.
* El color *"Dark-Cyan"*, previamente se ha dado de alta en Assets como `Color Set` y está definido por su código HEX.
* Los `ZStack` contendrán los elementos para generar los text fields con sus hint o placeholder, a continuación veremos el detalle:

```swift
// TextField (correo electrónico)
ZStack(alignment: .leading) {
    if correo.isEmpty {
        Text("ejemplo@gmail.com")
            .font(.caption)
            .foregroundColor(.gray)
    }
    
    TextField("", text: $correo)
}

...


// SecureField (contraseña)
ZStack(alignment: .leading) {
    if contrasena.isEmpty {
        Text("Escribe tu contraseña")
            .font(.caption)
            .foregroundColor(.gray)
    }
    
    SecureField("", text: $contrasena)
}
```
Recordando que en SwiftUI los text fields no tienen nativamente el hint, se tiene que generar mediante estos `ZStack`
para poder ocultarlos cuando el usuario escriba.

#### Estilos para botones
Para continuar con la elaboración de la pantalla de inicio, específicamente los botones de *Iniciar sesión*,
*Iniciar sesión con redes sociales*, *Facebook* y *Twitter*, revisaremos algunas cuestiones de estilos. Los elementos
siguientes los introduciremos en las estructuras `ScrollView` > `VStack` donde metimos los elementos anteriores.

##### Botón Iniciar sesión:
```swift
VStack {
    ...

    Button(action: {}, label: {
        Text("INICIAR SESIÓN")
            .fontWeight(.bold)
            .foregroundColor(.white)
            .frame(maxWidth: .infinity, alignment: .center)
            .padding(
                EdgeInsets(top: 11, leading: 18, bottom: 11, trailing: 18)
            )
            .overlay(
                RoundedRectangle(cornerRadius: 6.0)
                    .stroke(Color("Dark-Cyan"), lineWidth: 1.0)
                    .shadow(color: .white, radius: 1)
            )
    })
    
    ...
}
```

Donde resaltaremos el uso de los siguientes modificadores:
* `frame` con `maxWidth: .infinity`: para que el ancho de el frame se ajuste al total del ancho del padre.
* `EdgeInsets()`: para especificar el tamaño de cada uno de los paddings
* `overlay` > `RoundedRectangle`: que será un recatángulo redondeado que estará sobrepuesto a nuestro botón, en este caso
solo le estamos configurando un `stroke` para el color del borde y `shadow` para generar una sombra a este.

##### Botones Inicar sesión con redes sociales:
```swift
VStack {
    ...

    Text("Inicia sesión con redes sociales")
        .foregroundColor(.white)
        .frame(maxWidth: .infinity, alignment: .center)
        .padding(.top, 48.0)
        
    HStack {
        Button(action: {}, label: {
            Text("Facebook")
                .fontWeight(.bold)
                .foregroundColor(.white)
                .frame(width: 128, height: 48, alignment: .center)
                .overlay(
                    RoundedRectangle(cornerRadius: 6.0)
                        .foregroundColor(.white)
                        .opacity(0.2)
                )
        })

        Button(action: {}, label: {
            Text("Twitter")
                .fontWeight(.bold)
                .foregroundColor(.white)
                .frame(width: 128, height: 48, alignment: .center)
                .overlay(
                    RoundedRectangle(cornerRadius: 6.0)
                        .foregroundColor(.white)
                        .opacity(0.2)
                )
        })
    }
    .padding(.vertical)
    .frame(maxWidth: .infinity, alignment: .center)

...
}
```

Donde también hemos implementado el modificador `overlay` > `RoundedRectangle` para cada botón y, en
este caso el `frame(maxWidth: .infinity)` lo hemos implementado para un `HStack`.

### Vista de Registro
Para la vista de registro, importaremos solo un asset, que es la imagen ejemplo de perfil, notamos
que son pocas las diferencias con la pantalla de inicio se sesión, principalmente la imagen de perfil y el
campo de confirmación de contraseña, empezaremos por la primera:

#### Sección de imagen de perfil
Para empezar a trabajar, podemos colocar la variable `esInicioDeSesion = true`, de modo que nos muestre la
vista `RegistroView` por defecto, pues estaremos trabajando en ella.

![swift ui apps register design](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/swift-ui-apps-register.png?raw=true)

Nuestro `RegistroView` devolverá el siguiente `View`:

```swift
// RegistroView
var body: some View {
    ScrollView {
        VStack(alignment: .center) {
            Text("Elige una foto de perfil")
                .fontWeight(.bold)
                .foregroundColor(.white)

            Text("Puedes cambiar o elegirlo más adelante")
                .font(.footnote)
                .fontWeight(.light)
                .foregroundColor(.gray)
                .padding(.bottom)

            Button(action: tomarFoto, label: {
                ZStack {
                    Image("profile-example")
                        .resizable()
                        .aspectRatio(contentMode: .fill)
                        .frame(width: 80, height: 80)

                    Image(systemName: "camera")
                        .foregroundColor(.white)
                }
            }).padding(.bottom)
        }
    }
}
```

Nótese que el botón para agregar foto de perfil se compone de dos imágenes sobrepuestas que indican
que con el botón se abriría la cámara.

#### Forms del registro
Los campos de texto para esta vista son muy parecidos a los de la vista de registro, por lo cuál,
podemos copiarlos y generar un nuevo campo, así como una nueva variable bindeada a este:

```swift
// RegistroView
var body: some View {
    ScrollView {
        // VSTACK IMAGEN PERFIL

        VStack(alignment: .leading) {
            // Correo
            Text("Correo electrónico*")
                .foregroundColor(Color("Dark-Cyan"))
            
            ZStack(alignment: .leading) {
                if correo.isEmpty {
                    Text("ejemplo@gmail.com")
                        .font(.caption)
                        .foregroundColor(.gray)
                }
                TextField("", text: $correo)
            }

            Divider().frame(height: 1)
                .background(Color("Dark-Cyan"))
                .padding(.bottom)

            // Contraseña
            Text("Contraseña")
                .foregroundColor(.white)

            ZStack(alignment: .leading) {
                if contrasena.isEmpty {
                    Text("Escribe tu contraseña")
                        .font(.caption)
                        .foregroundColor(.gray)
                }
                SecureField("", text: $contrasena)
            }

            // Confirmar contraseña
            Text("Confirmar contraseña")
                .foregroundColor(.white)

            ZStack(alignment: .leading) {
                if confirmarContrasena.isEmpty {
                    Text("Vuelve a escribir tu contraseña")
                        .font(.caption)
                        .foregroundColor(.gray)
                }
                SecureField("", text: $confirmarContrasena)
            }

            Divider().frame(height: 1)
                .background(Color("Dark-Cyan"))
                .padding(.bottom)
        }
        .padding(.horizontal, 77.0)
    }
}
```
Nótese que en este caso no hemos agregado los botones en este `VStack`, ya que de hacerlo XCode no
nos permitiría agregar ese número de elementos dentro de un stack. Por ello, generaremos otro para los
botones:

#### Botones del registro
Ahora, dentro de un nuevo `VStack` pondremos exactamente los mismos botones de la sección de Inicio de sesión:
```swift
// RegistroView
var body: some View {
    ScrollView {
        // VStack IMAGEN PERFIL

        // VStack FORMS

        VStack {
            Button(action: {}, label: {
                Text("REGÍSTRATE")
                    .fontWeight(.bold)
                    .foregroundColor(.white)
                    .frame(maxWidth: .infinity, alignment: .center)
                    .padding(
                        EdgeInsets(top: 11, leading: 18, bottom: 11, trailing: 18)
                    )
                    .overlay(
                        RoundedRectangle(cornerRadius: 6.0)
                            .stroke(Color("Dark-Cyan"), lineWidth: 1.0)
                            .shadow(color: .white, radius: 1)
                    )
            })

            Text("Inicia sesión con redes sociales")
                .foregroundColor(.white)
                .frame(maxWidth: .infinity, alignment: .center)
                .padding(.top, 48.0)

            HStack {
                Button(action: {}, label: {
                    Text("Facebook")
                        .fontWeight(.bold)
                        .foregroundColor(.white)
                        .frame(width: 128, height: 48, alignment: .center)
                        .overlay(
                            RoundedRectangle(cornerRadius: 6.0)
                                .foregroundColor(.white)
                                .opacity(0.2)
                        )
                })

                Button(action: {}, label: {
                    Text("Twitter")
                        .fontWeight(.bold)
                        .foregroundColor(.white)
                        .frame(width: 128, height: 48, alignment: .center)
                        .overlay(
                            RoundedRectangle(cornerRadius: 6.0)
                                .foregroundColor(.white)
                                .opacity(0.2)
                        )
                })
            }
            .padding(.vertical)
            .frame(maxWidth: .infinity, alignment: .center)
        }
        .padding(.horizontal, 77.0)
    }
}
```

# Ruteo de la aplicación
Para generar la navegación desde la pantalla de login hacia las pantallas subsecuentes de la app,
vamos a embeber el `ZStack` que habíamos puesto como única vista de `ContentView` dentro de un
`NavigationView`:
```swift
struct ContentView: View {
    var body: some View {
        NavigationView {
            ZStack { ... }
            .navigationBarHidden(true)
        }
    }
}
```

Nótese que estamos implementando el modificador `navigationBarHidden` para ocultar el header que este
tipo de contenedor nos genera de manera automática.

## Navegación del login al home
Para generar la navegación del login hacia la pantalla de inicio, tenemos que generar una nueva
variable de estado `isHomePageActive` para controlar si la página `Home` está activa:
```swift
struct InicioSesionView: View {
    @State var correo = ""
    @State var contrasena = ""
    @State var isHomePageActive = false
    
    var body: some View {
        ScrollView {
            VStack(alignment: .leading) {
                ...

                Button(action: {
                    iniciarSesion()
                }, label: {
                    Text("INICIAR SESIÓN")
                    ...
                })
                
                ...
            }
            .padding(.horizontal, 77.0)
            
            NavigationLink(
                destination: Home(),
                isActive: $isHomePageActive,
                label: {
                    EmptyView()
                })
        }
    }
    
    // Cambia variable de estado para generar la navegación
    func iniciarSesion() {
        isHomePageActive = true
    }
}
```
Nótese la implemantación de la estructura `NavigationLink`, donde indicamos la página
destino en `destination`, la variable que controla el navigation `isActive` y un `label`, que
en este caso le pasaremos un `EmptyView` ya que no requerimos el link pues el `Button` es la
cara visible de esta acción.

## Pantalla Home
Para la pantalla `Home`, como se indicó en el `NavigationLink`, generaremos otra estructura en este caso,
dentro de otro archivo que llamaremos `Home.swift`. Dentro de esta vista, vamos a hacer uso de un `TabView`,
para switchear entre diferentes pantallas que hemos dedefinir más adelante. Dentro del `TabView`, incluiremos
cuatro vistas de manera provisional (`Text`), agregaremos un poco de estilos y el modificador `tabItem` para
que se conviertan en las tabs y para que en el footer podamos visualizar los botones que nos permitan switchear
entre las cuatro diferentes vistas:

Diseño tabs:

![swift ui apps tabs](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/swift-ui-apps-tabs.png?raw=true)

```swift
struct Home: View {
    var body: some View {
        TabView {
            Text("Perfil")
                .font(.system(size: 30, weight: .bold, design: .rounded))
                .tabItem {
                    Image(systemName: "person")
                    Text("Perfil")
                }
            
            Text("Juegos")
                .font(.system(size: 30, weight: .bold, design: .rounded))
                .tabItem {
                    Image(systemName: "gamecontroller")
                    Text("Juegos")
                }
            
            Text("Pantalla home")
                .font(.system(size: 30, weight: .bold, design: .rounded))
                .tabItem {
                    Image(systemName: "house")
                    Text("Home")
                }
            
            Text("Favoritos")
                .font(.system(size: 30, weight: .bold, design: .rounded))
                .tabItem {
                    Image(systemName: "heart")
                    Text("Favoritos")
                }
        }
        
    }
}
```

### Poner un tab por defecto
Para establecer un tab como seleccionado por default en el `TabView`, tenemos que generarle a cada
vista un `tag`, ejemplo:
```swift
TabView {
    Text("Perfil")
        .font(.system(size: 30, weight: .bold, design: .rounded))
        .tabItem {
            Image(systemName: "person")
            Text("Perfil")
        }
        .tag(0)
    
    ...
}
```

De esta forma le configuraremos un `tag` a cada vista, porque posteriormente configuraremos una variable
en el `@State` que almacene el tab seleccionado:
```swift
struct Home: View {
    @State var selectedTab: Int = 2
    
    var body: some View {
        TabView(selection: $selectedTab) { ... }
    }
}
```
Nótese que estamos colocando el tab seleccionado como `2`, de esta forma nuestra *Pantalla home* será la
tab por defecto.

### Cambiar estilos de la barra de tabs
Para modificar los estilos de los tabs que ve el usuario en la parte posterior, implementaremos el método
`init`, que forma parte del ciclo de vida de nuestra vista, para que en el momento en el que esta se instancia,
se modifiquen los estilos.

Para esto, también haremos uso de la librería que se usaba anterior a SwiftUI, que era `UIKit`, ésta poseía
la clase `UITabBar`:
```swift
    init() {
        UITabBar.appearance()
            .barTintColor = UIColor((Color("Tabbar-Color")))
        UITabBar.appearance()
            .isTranslucent = true
    }
```
Estos estilos se compilarán en nuestros tab bars, en este caso, el color de la barra `appearance().barTintColor`
y una pequeña opacidad `appearance().isTranslucent`. (El color se ha definido en nuestros assets, R: 57, G: 63, B: 83).

Finalmente, para establecer el color de los items de cada tab, configuraremos un modificador a la vista `TabView`:
```swift
TabView(selection: $selectedTab) { ... }
    .accentColor(.white)
```

### Tab home
Diseño de la tab Home:

![swift ui apps Home](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/swift-ui-apps-home.png?raw=true)

Hemos de generar la vista de la tab home, en un módulo separado:
```swift
struct PantallaHome: View {
    var body: some View {
        Text("Hola")
    }
}
```

Posteriormente, sustituiremos en lugar del texto que teníamos colocado en la tab:
```swift
    PantallaHome()
        .font(.system(size: 30, weight: .bold, design: .rounded))
        .tabItem {
            Image(systemName: "house")
            Text("Home")
        }
        .tag(2)
```

#### Estructura inicial
Empezaremos a construir nuestra pantalla, configurando los stacks correspondientes, el color de
fondo y un contenido inicial:
```swift
struct PantallaHome: View {
    var body: some View {
        ZStack {
            Color("Navy").ignoresSafeArea()
            
            VStack {
                // Logo
                // Barra de búsqueda

                ScrollView {
                // Sub secciones del home
                }

            }
            .padding(.horizontal, 18)
        }
        .navigationBarHidden(true)
        .navigationBarBackButtonHidden(true)
    }
}
```
En este caso, hemos implementado un `ZStack` dentro de un `ScrollView` que nos permita posteriormente generar contenido
scrolleable de manera vertical; dos modificadores al `ZStack`, que son `navigationBarHidden` y 
`navigationBarBackButtonHidden` para que no se visualice ni la barra de navegación, ni el botón para hacerla navegación nativos.

#### Logo
Para configurar el logo, tomaremos el logo que habíamos implementado en `ContentView`:
```swift
    VStack {
        Image("app_logo")
            .resizable()
            .aspectRatio(contentMode: .fit)
            .frame(width: 250)
            .padding(.horizontal, 11.0)
    }
    .padding(.horizontal, 18)
```

#### Barra de búsqueda
Para la barra de búsqueda, implementaremos un `HStack` después del logo, donde estará principalmente nuestro ícono
de búsqueda a modo de botón y el `TextField` con un text hint que se muestre/oculte dependiendo si está vacío el
campo de texto o no, dependiento de la variable de estado `textoBusqueda`:
```swift
    VStack {
        ...

        HStack {
            // Si no hay texto buscado, el ícono se verá amarillo
            Button(action: busqueda, label: {
                Image(systemName: "magnifyingglass")
                    .foregroundColor(textoBusqueda.isEmpty ? Color(.yellow) : Color("Dark-Cyan"))
            })
            // Si no hay texto buscado, se verá el hint "Buscar un video" de un color grisáceo
            ZStack(alignment: .leading) {
                if textoBusqueda.isEmpty {
                    Text("Buscar un video")
                        .foregroundColor(Color(red: 174/255, green: 177/255, blue: 185/255, opacity: 1.0))
                }
                // Campo de texto
                TextField("", text: $textoBusqueda)
                    .foregroundColor(.white)
            }
        }
        .padding([.top, .leading, .bottom], 11.0)
        .background(Color("Tabbar-Color"))
        .clipShape(Capsule())
    }
    .padding(.horizontal, 18)
```
El modificador `clipShape(Capsule())`, nos permite hacer que nuestro contenedor tome la forma de una cápsula
(bordes redondeados).

También definiremos a nivel de nuestra estructura `PantallaHome` la variable de estado `textoBusqueda` y
la función `busqueda`:
```swift
struct PantallaHome: View {
    @State var textoBusqueda = ""

    var body: some View {...}

    func busqueda() {
        print("El usuario está buscando \(textoBusqueda)")
    }
}
```

#### Sección "Los más populares"
Para la vista de la parte superior, generaremos un submódulo llamado `SobModuloHome`, que se indexará al mismo
nivel que nuestro logo y nuestra barra de búsqueda para luego insertarle las subsecciones de nuestro home:
```swift
struct PantallaHome: View {
    var body: some View {
        ZStack {
            Color("Navy").ignoresSafeArea()
            
            
            VStack {
                Image("app_logo")...

                HStack {
                    // Barra de búsqueda
                }
                .padding([.top, .leading, .bottom], 11.0)
                .background(Color("Tabbar-Color"))
                .clipShape(Capsule())

                ScrollView {
                    SobModuloHome()
                }
            }
            .padding(.horizontal, 18)
        }
        .navigationBarHidden(true)
        .navigationBarBackButtonHidden(true)
    }
}

struct SobModuloHome: View {
    var body: some View {
        // Contenido de la vista
    }
}
```

##### Variables
Primeramente, configuraremos un par de variables de estado que servirán para las urls de los videos que se pueden
reproducir desde esta vista:
* `url` como url actual
* `isPlayerActive` que nos permite identificar si se ha abierto el reproductor
* `urlVideos` colección de las urls de todos los videos
```swift
struct SobModuloHome: View {
    @State var url = "https://cdn.cloudflare.steamstatic.com/steam/apps/256658589/movie480.mp4"
    @State var isPlayerActive = false
    let urlVideos:[String] = [
        "https://cdn.cloudflare.steamstatic.com/steam/apps/256658589/movie480.mp4",
        "https://cdn.cloudflare.steamstatic.com/steam/apps/256671638/movie480.mp4",
        "https://cdn.cloudflare.steamstatic.com/steam/apps/256720061/movie480.mp4",
        "https://cdn.cloudflare.steamstatic.com/steam/apps/256814567/movie480.mp4",
        "https://cdn.cloudflare.steamstatic.com/steam/apps/256705156/movie480.mp4",
        "https://cdn.cloudflare.steamstatic.com/steam/apps/256801252/movie480.mp4",
        "https://cdn.cloudflare.steamstatic.com/steam/apps/256757119/movie480.mp4"
    ]
    
    var body: some View {}
}
```

##### Título
Para el título, iniciaremos indexando un texto dentro de un `VStack` que contendrá todos nuestros elementos:
```swift
var body: some View {
    VStack {
        Text("LOS MÁS POPULARES")
            .font(.title3)
            .foregroundColor(.white)
            .bold()
            .frame(minWidth: 0, maxWidth: .infinity, alignment: .leading)
    }.padding(.top)
}
```

##### Miniatura
Para la miniatura, vamos a generar un `ZStack` con la imagen, el ícono de reproducir y el pequeño label:
```swift
var body: some View {
    VStack {
        Text("LOS MÁS POPULARES")...
        
         ZStack {
             // La acción será seleccionar la url en la posición 0 y activar el player
            Button(action: {
                url = urlVideos[0]
                print("URL: \(url)")
                isPlayerActive = true
            }, label: {
                VStack(spacing: 0) {
                    Image("The Witcher 3")
                        .resizable()
                        .scaledToFill()
                    
                    Text("The Witcher 3")
                        .frame(minWidth: 0, maxWidth: .infinity, alignment: .leading)
                        .background(Color("Navy"))
                }
            })
            Image(systemName: "play.circle.fill")
                .resizable()
                .foregroundColor(.white)
                .frame(width: 42, height: 42)
        }
        .frame(minWidth: 0, maxWidth: .infinity, alignment: .center)
        .padding(.vertical)
    }.padding(.top)
}
```
Notemos que básicamente tenemos un botón con el ícono de reproducir superpuesto, el `label` de este botón,
se compone a su vez de una Imagen (la miniatura) y un pequeño texto.

##### Reproductor de video
Finalmente, con la finalidad de que la acción del botón que hemos configurado nos direccione al reproductor,
generaremos un `NavigationLink` para que este se abra en una nueva ruta, para ello, primeramente importaremos
`AVKit` al inicio de nuestro archivo:
```swift
import SwiftUI
import AVKit
```

Posteriormente, pondremos nuestro `NavigationLink` a la altura del `VStack` principal de la vista `SobModuloHome`:
```swift
var body: some View {
    VStack {...}.padding(.top)

    NavigationLink(
        destination: VideoPlayer(player: AVPlayer(url: URL(string: url)!))
            .frame(width: 400, height: 300),
        isActive: $isPlayerActive,
        label: {
            EmptyView()
        }
    )
}
```
Donde notaremos tres detalles:
* El `AVPlayer` recibe como argumento la url de nuestra variable de estado pero es necesario colocarle el signo `!'
para responsabilizarnos de que ésta valor será válida.
* La vista se activa mediante la variable de estado `isPlayerActive` habilitada por el botón de la miniatura.
* No lleva label, por ello se implementa el `EmptyView`.

#### Sección "Categorías sugeridas para ti"
Para esta sección, continuaremos agregand contenido a la misma altura del `VStack` principal del `SobModuloHome`:

##### Título
Será prácticamente idéntico al de la sección anterior:
```swift
var body: some View {
    VStack {
        Text("LOS MÁS POPULARES")...

        ZStack {...}

        Text("CATEGORIAS SUGERIDAS PARA TI")
            .font(.title3)
            .foregroundColor(.white)
            .bold()
            .frame(minWidth: 0, maxWidth: .infinity, alignment: .leading)
    }
}
```

##### Carousel
Para el carrusel que muestra los íconos, haremos un `ScrollView` horizontal que despliegue botones en forma
de rectángulos con una imagen adentro, para ello implementaremos el siguiente `HStack`:
```swift
var body: some View {
    VStack {
        ...

        Text("CATEGORIAS SUGERIDAS PARA TI")
            .font(.title3)
            .foregroundColor(.white)
            .bold()
            .frame(minWidth: 0, maxWidth: .infinity, alignment: .leading)

        ScrollView(.horizontal, showsIndicators: false) {
            HStack {
                Button(action: {}, label: {
                    ZStack {
                        RoundedRectangle(cornerRadius: 8.0)
                            .fill(Color("Tabbar-Color"))
                            .frame(width: 160, height: 90)
                        
                        Image("fps")
                            .resizable()
                            .scaledToFill()
                            .frame(width: 42, height: 42)
                    }
                })
                    
                Button(action: {}, label: {
                    ZStack {
                        RoundedRectangle(cornerRadius: 8.0)
                            .fill(Color("Tabbar-Color"))
                            .frame(width: 160, height: 90)
                        
                        Image("rpg")
                            .resizable()
                            .scaledToFill()
                            .frame(width: 42, height: 42)
                    }
                })
                    
                Button(action: {}, label: {
                    ZStack {
                        RoundedRectangle(cornerRadius: 8.0)
                            .fill(Color("Tabbar-Color"))
                            .frame(width: 160, height: 90)
                        
                        Image("open-world")
                            .resizable()
                            .scaledToFill()
                            .frame(width: 42, height: 42)
                    }
                })
            }
        }
    }
}
```

Notemos que dentro del `ScrollView` con la opción horizontal, solo hemos generado un `HStack` en el cual
hemos añadido nuestros elementos que han de hacer scroll de manera horizontal.

#### Sección "Recomendados para ti"
Para esta sección, continuaremos agregand contenido a la misma altura del `VStack` principal del `SobModuloHome`:

##### Título
Será prácticamente idéntico al de la sección anterior:
```swift
var body: some View {
    VStack {
        Text("LOS MÁS POPULARES")...

        ZStack {...}

        Text("CATEGORIAS SUGERIDAS PARA TI")...

        ScrollView(.horizontal, showsIndicators: false) {...}

        Text("RECOMENDADOS PARA TI")
            .font(.title3)
            .foregroundColor(.white)
            .bold()
            .frame(minWidth: 0, maxWidth: .infinity, alignment: .leading)
    }
}
```

##### Carousel
El carrusel de esta sección se hará bajo la misma lógica del anterior, solo usando otras imágenes y prgramando
los `action` de manera que desplieguen el reproductor de video a partir de las urls 1, 2 y 3:
```swift
var body: some View {
    VStack {
        ...

        Text("RECOMENDADOS PARA TI")
            .font(.title3)
            .foregroundColor(.white)
            .bold()
            .frame(minWidth: 0, maxWidth: .infinity, alignment: .leading)

        ScrollView(.horizontal, showsIndicators: false) {
            HStack {
                Button(action: {
                    url = urlVideos[1]
                    print("URL: \(url)")
                    isPlayerActive = true
                }, label: {
                    Image("Abzu")
                        .resizable()
                        .scaledToFill()
                        .frame(width: 240, height: 135)
                })
                    
                Button(action: {
                    url = urlVideos[2]
                    print("URL: \(url)")
                    isPlayerActive = true
                }, label: {
                    Image("Crash Bandicoot")
                        .resizable()
                        .scaledToFill()
                        .frame(width: 240, height: 135)
                })
                    
                Button(action: {
                    url = urlVideos[3]
                    print("URL: \(url)")
                    isPlayerActive = true
                }, label: {
                    Image("DEATH STRANDING")
                        .resizable()
                        .scaledToFill()
                        .frame(width: 240, height: 135)
                })
            }
        }
    }
}
```