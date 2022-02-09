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

# Primeras pantallas
Para empezar a crear nuestro proyecto, seleccionaremos en Xcode, Crear nuevo proyecto > iOS > App >
Pondremos nombre, ejemplo: `GameStream`. No será necesario seleccionar o mdificar ninguna otra opción.
Posteriormente localizaremos el proyecto en la ubicación que requiramos, preferentemente aceptando
que se cree el repo local.

## Pantalla de inicio
Para la pantalla de login, observamos que es necesario importar primeramente algunos aassets, principalmente
el logotipo.

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

### Lógica de pantalla de inicio
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

### SecureField y Scroll
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