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

# Primeras pantallas
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