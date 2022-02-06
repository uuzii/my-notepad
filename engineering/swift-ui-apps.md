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

## Planear una aplicación
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