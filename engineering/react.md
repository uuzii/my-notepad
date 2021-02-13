# React
Una librería creada por Facebook para optimizar el desarrollo Front.

# Virtual DOM
Es una copia del DOM en la cuál podemos identificar qué elementos del DOM cambia para no re renderizar todo el DOM, sino solo la sección en la que está contenida el elemento cambiante.

# Create React App
Con el siguiente comando podremos generar un boilerpalte con toda la configuraicón un proyecto react para empezar a trabajar:
```bash
npx create-react-app [name]
```

Para ejecutarlo, entramos a la carpeta generada y ejecutamos `npm run start`, nos abrirá un navegador con el proyecto inicializado (solo una imagen y un enlace a la documentación de React).

# Tipos de componentes
* **Componente stateful**. Es conocido como *estructura de clases*, nos permite manejar ciclo de vida y estado, es el componente más poderoso de React, su estructura básica es la siguiente:
```jsx
import React, {Component} from 'react';

class Stateful extends Component {
  constructor(props) {
    super(props);
    this.state = {
      hello: 'Hola mundo!'
    }
  }
  // parte visual de nuestro componente
  render() {
    return(
      <h1>{this.state.hello}</h1>
    )
  }
}

export default Stateful;
```

* **Componente stateless**. No depende de un estado ni de un ciclo de vida, solamente la parte lógica. Estos componentes son muy usados actualmente pues se enfocan en ciertas partes de la funcionalidad. Esta es su estructura:
```javascript
import React from 'react';

const Stateless = () => {
  return(
    <h1>Hola mundo</h1>
  );
}

export default Stateless;
```

* **Componente presentacional**. No contiene ninguna lógica ni estado, solamente la parte visual, estructura básica:
```javascript
import React from 'react';

const Presentacional = () => <h1>Hola mundo!</h1>

export default Presentacional;
```

# JSX
Es una extensión que podemos usar para nuestros componentes React, que nos brina una interfaz con utilidades en tiempo de desarrollo, si bien mantiene el resaltado de JS, también nos permite escribir más fácilmente nuestros componentes de React así como auxiliares de linteo, veamos un ejemplo de un componente con jsx:
```jsx
import React from 'react'

const HolaMundo = () => {
  const Hello = 'Hola mundo!'
  const isTrue = true
  return(
    <div className="HolaMundo">
      <h1>{Hello}</h1>
      <h2>Curso de React</h2>
      <input type="text"/>
      {isTrue ? <h4>Esto es verdadero</h4> : <h4>Soy falso</h4>}
      {isTrue && <h4>Soy verdadero</h4>}
    </div>
  )
}

export default HolaMundo;
```
> Tomar en cuenta que las etiquetas que no tienen cierre, se tiene que indicar ejemplo <input type="text"/>

# Props: comunicación entre componentes
Nuestros componentes se pueden alimentar de información desde el exterior mediante los argumentos que reciben al crearlos, por definición es bueno mandarlos mediante un argumento llamado `props``
```jsx
import React from 'react'

const Button = props => {
  const {text} =  props
  return(
    <div>
      <button type="button">{text}</button>
    </div>
  )
};

export default Button;
```

> Nótese que resulta simple utilzar `text` de manera simple ya que hemos destructurado el objeto props.


La implementación se vería algo así:
```javascript
ReactDOM.render(
  <Button text="Click 2" />,
  document.getElementById('root')
);
```

# Ciclo de vida de los componentes React
Los componentes pasan por una serie de fases de ciclo de vida. Algunos serán toda una sección del componente y otros se pueden mandar llamar para asignar actividades según las necesidades, las fases se pueden clasificar de la siguiente manera:

* Montaje. El componete se crea junto a la lógica y los componentes internos, luego son insertados en el DOM. Métodos:
  * **constructor()** es el primer método llamado, aquí se inicializan métodos, controladores y eventos del estado.
  * **getDerivedStateFromProps()** se llama antes de presentarse en el DOM, nos permite actualizar el estado interno en respuesta a un cambio en las propiedades.
  * **render()** se llama para representar los elementos del DOM e implementar su lógica
  * **componentdidMount()** se llama cuando el componente ha sido montado en el DOM, aquí se trabajan los eventos que permiten interactuar con el componente.
* Actualización. Nuestro componente está pendiente de los cambios que pueden presentarse en el state o las props para hacer los cambios que sean necesarios.
  * **getDerivedStateFromProps()** es el primero en ejecutarse en la fase de actualización y funciona de la misma forma que en el montaje
  * **shouldComponentUpdate()** en él se puede controlar la fase de actualización, retornamos un booleano para proceder o no con la actualización, se usa para optimización.
  * **render()** se llama para representar los cambios en el DOM
  * **componentDidUpdate()** es invocado inmediatamente después de que el componente se actualiza y recibe compo argumentos las propiedades y el estado.
* Demsontaje. Nuestro componente "muere" cuando no necesitamos más un elemento de nuestra aplicación, esto es, sacarlo del DOM.
  * **componentWillUnmount()** se llama justo antes de que el componente sea eliminaddo del DOM
* Manejo de errores. Cuando se sucita algún error en el código de la aplicación, se entra en esta fase para entender qué está ocurriendo.
  * **getDerivedStateFromError()** se llama tan pronto se sucita un error, recibe como argumento el error.
  * **componentDidCatch()** se llama después de lanzarse un error, recibe como argumento el error

En la siguiente [liga](https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/), encontramos un diagrama ilustrativo de las primeras tres:

# Instlación y configuración de un proyecto
Para empezar a configurar un proyecto desde cero, creamos nuestra carpeta, inicializamos nuestro repo local e inicializamos npm:
```bash
mkdir [project]
git init
npm init
```

Con lo cuál tendremos solamente nuestro `package`, vamos a crear la estrucutra básica nuestro proyecto:
```bash
project/
--- public/
--- --- index.html
--- src/
--- --- components/
--- --- index.js
```

Ahora instalamos las dependencias react y react-dom con el siguiente comando:
```bash
npm install react react-dom
```

## Agregando componentes al proyecto
Dentro de la carpeta `components, creamos un archivo llamado `HelloWorld.jsx` con la siguiente estructura:
```jsx
import React from 'react'

const HelloWorld = () => (
  <h1>hola mundo!</h1>
)

export default HelloWorld
```

## Agregando el entrypoint de nuestro proyecto
El entrypoint es el primer archivo que será leído en nuestro proyecto, como su njombre lo dice: punto de entrada, en él haremos uso de React DOM, que nos permitirá implementar nuestro código jsx en el proyecto. Todo esto lo haremos en el archivo `index.js`:
```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import Helloworld from './components/HelloWorld'
// La función render, recib el componente a mostrar y el nodo en el cuál se va indexar
ReactDOM.render(<Helloworld />, document.getElementById('app'))
```

## Agregando el html de nuestro proyecto
Este será el markup principal de nuestro proyecto, donde generaremos el elemento donde será renderizada uestra aplicación:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Project</title>
</head>
<body>
  <div id="app"></div>
</body>
</html>
```

## Agregando compatibilidad con Babel
Babel es una herramienta que utilizaremos para trnaspilar nuestro código moderno de javascript a código que todos los navegadores puesan soportar.

Primeramente instalamos las dependencias que necesitamos para babel como dependencias de desarrollo:
```bash
npm install @babel/core babel-loader @babel/preset-env @babel/preset-react --save-dev
```

Creamos el archivo `.babelrc` en la raíz de nuestro proyecto con la siguiente configuración:
```json
{
  "presets": [
    "@babel/preset-env",
    "@babel/preset-react"
  ]
}
```

## Configurando webpack
Webpack nos ayudará a preparar nuestro proyecto para el entorno de desarrollo local y eventualmente para su despliegue a producción: reunirá nuestros archivos y los optimizará.

Primeramente instalamos las siguientes dependencias de desarrollo:
```bash
npm install webpack webpack-cli html-webpack-plugin html-loader --save-dev
```

Ahora generamos en la raíz de nuestro proyecto el archivo `webpack.config.js` con la siguiente estructura:
```javascript
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  resolve: {
    extensions: ['.js', '.jsx']
  },
  module: {
    rules: [
      {
        test: /\.(js|jsx)$/,
        exclude: /node_modules/,
        use: {
          loader: "babel-loader"
        }
      },
      {
        test: /\.html$/,
        use: {
          loader: "html-loader"
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './public/index.html',
      filename: './index.html'
    })
  ]
}
```

Donde:
* ´entry´ contiene información sobre el punto de acceso
* ´output´ de donde ubicará y cómo nombrará los archivos resultado del proceso de compilación
* ´resolve´ para indicar las extensiones que se resolverán
* ´module´ contiene las reglas que seguirá nuestro proyecto

Posteriormente agregaremos un nuevo script a nuestro ´package´:
```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --mode production"
  },
```

Ahora corremos la compilación con
```bash
npm run build
```
> Nótese que esta configuración es útil para generar un bundle de nuestro proyecto para producción.


Ahora veremos como resultado nuestra carpeta dist con todo nuestro proyecto preparado para producción en la carpeta de `dist`.

### Webpack dev server
Para poder probar lo que estamos construyendo también nos apoyaremos de WebPack para generar un server local en donde podamos ejecutar nuestro proyecto para develop.

Primeramente instalamos la dependencia:
```bash
npm install webpack-dev-server --save-dev
```

Generaremos otro script en nuestro `package`:
```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --mode production",
    "start": "webpack serve --mode development --env development"
  },
```

Corremos el comando con:
```bash
npm run start
```

Con esto ya podremos ver nuestro proyecto en un entorno de desarrollo local que se actualizará cada que hagamos un cambio.

## Configurando sass
Para configurar sass en nuestro proyecto tenemos que llevar a cabo la siguiente configuración:

Instalamos las siguientes dependencias de desarollo:
```bash
npm install mini-css-extract-plugin css-loader node-sass sass-loader --save-dev
```

Agregamos una nueva dependencia `MiniCssExtractPlugin`, así como una nueva regla y un nuevo plugin en nuestro archivo `webpack.config.js`:
```javascript
...
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
  module: {
    rules: [
      ...
      {
        test: /\.(s*)css$/,
        use: [
          {
            loader: MiniCssExtractPlugin.loader,
          },
          'css-loader',
          'sass-loader'
        ]
      }
    ],
    plugins: [
      ...
      new MiniCssExtractPlugin({
        filename: 'assets/[name].css'
      })
    ]
  }
}
```

Adicionalmente agregaremos a nuestro proyecto una llamada `assets` dentro de la carpeta `src`, que será donde vivirán nuestros archivos `sass` así como cualquier recurso que se use en nuestro proyecto.
```bash
--- src/
--- assets/
--- --- App.scss
```

Inicialmente vamos a crear también un archivo llamado `App.scss` para generar nuestros primero estilos:
```scss
body {
  margin: 0;
  background: red;
}
```

Para implementarlo en nuestro componente tendríamos que importarlo
```jsx
// HelloWorld.jsx
import '../assets/styles/App.scss'
```

Con sto ya podríamos generar nuestros estilos en archivos sass para que luego sean agregados en la compilación de webpack.

## Configurando ESLint
ESLint es una herramienta que nos permitirá asegurarnos de que nuestro código está escrito con la sintaxis correcta en este caso de jsx.

Para empezar a configurarlo en nuestro proyecto instalamos las siguientes dependencias de desarrollo:
```bash
npm install eslint babel-eslint eslint-config-airbnb eslint-plugin-import eslint-plugin-react eslint-plugin-jsx-a11y
```

Una vez instaladas las dependencias, agregaremos el contenido del siguiente [gist](https://gist.github.com/gndx/60ae8b1807263e3a55f790ed17c4c57a) a un archivo llamado `.eslintrc`.

## Generando un gitignore
Este archivo nos ayudará para que no se agreguen a nuestro repositorio todas las dependencias: crearemos un archivo llamado `.gitignore` con el contenido de este [gist](https://gist.github.com/gndx/747a8913d12e96ff8374e2125efde544).

# Construyendo nuestra aplicación estilo Netflix:

## Descomponetización
Vamos a obedecer la siguiente estructura de componentes para nuestra aplicación:
![componentes](https://github.com/uuzii/my-notepad/blob/wip/engineering/engineering/assets/componentes.jpeg?raw=true)

## Añadiendo componentes
Generemos un componente presentacional llamado header en `src/components` con el markup adecuado para éste. Para estructurar mejor nuestra aplicación, generaremos un archivo llamado `App.jsx` en `src/containers/`, mismo que funcionará como el *wrapper* principal de toda nuestra aplicación, en él iremos importando nuestro componente `Header`:
```jsx
// App.jsx
import React from 'react'
import Header from '../components/Header'

const App = () => (
  <div className="App">
    <Header />
  </div>
)

export default App
```

> Nótese que los componentes presentacionales solo pueden recibir un elemento
> Nótese que siempre usaremos el atributo `className` en jsx en lugar de `class`

Luego en el `index.js`, importaremos este único elemento:
```javascript
// index,js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './containers/App'
// La función render, recib el componente a mostrar y el nodo en el cuál se va indexar
ReactDOM.render(<App />, document.getElementById('app'))
```

Para agregarle estilos a este componente, generamos un archivo `App.scss` dentro de `src/assets/styles/`, mismo que llenaremos con los estilos adecuados e importaremos en nuestro componente de la siguiente manera:
```jsx
// App.jsx
import '../assets/styles/App.scss'
```

Para agregarle estilos a nuestro componente ´Header´, nos conviene generar una jerarquía también dentro de assets, la cuál sería `src/assets/styles/components/` y le colocaríamos el mismo nombre del componente que estamos estilizando solo que con la extensión scss, en este caso sería `Header.scss`. La manera de importarlo en su respectivo componente es la misma que con el contenedor `App`.

Hasta este momento, nuestra carpeta `src` tendría la siguiente estructura:
```bash
src/
--- assets/
--- --- styles/
--- --- --- App.scss
--- --- --- components/
--- --- --- --- Header.scss
--- components/
--- --- Header.jsx
--- containers/
--- --- App.jsx
```

Esta misma estructura seguiremos para agregar los demás componentes de nuestro diseño como son el `Search` y el `Footer`. Para el `Carousel` haremos una implementación más detallada, ya que observamos que en el visual tenemos dos categorías de videos, pero realmente son dos elementos del mismo componente, por ello crearemos un componente llamado `Categories` que tendrá una estructura interna como la siguiente:
```jsx
// Categories.jsx
import React from 'react'

const Categories = ({ children }) => (
  <div className="categories">
    <h3 className="categories__title">Mi lista</h3>
    {children}
  </div>
)

export default Categories
```

Donde children, será un *slot* que será capaz de recibir otros componentes agregándoles un título. Consecuentemente, nuestro componente `Carousel` será también un wraper de lo que será un `Item`:
```jsx
// Carousel.jsx
import React from 'react'

const Carousel = ({ children }) => (
  <section className="carousel">
    <div className="carousel__container">
      {children}
    </div>
  </section>
)

export default Carousel
```

Dados estos componentes que tienen los slots llamados `children`, podemos generar la siguiente anidación en nuestra app (después de importarlos):
```jsx
import React from 'react'
import Header from '../components/Header'
import Search from '../components/Search'
import Categories from '../components/Categories'
import Carousel from '../components/Carousel'
import CarouselItem from '../components/CarouselItem'
import Footer from '../components/Footer'
import '../assets/styles/App.scss'


const App = () => (
  <div className="App">
    <Header />
    <Search />
    <Categories>
      <Carousel>
        <CarouselItem />
      </Carousel>
    </Categories>
    <Footer/>
  </div>
)

export default App
```

## Añadiendo imágenes con Webpack
Para agregar nuestras imágenes en nuestro proyecto y sean tomadas en cuenta a la hora de la compilación de webpack, tenemos que hacer la siguiente configuración:

Instalaremos las siguientes dependencias de desarrollo:
```bash
npm install file-loader --save-dev
```

Agregamos la siguiente regla a nuestro archivo de configuración de webpack:
```javascript
...
{
  test: /\.(png|gif|jpg)$/,
  use: [
    {
      loader: 'file-loader',
      options: {
        name: 'assets/[hash].[ext]'
      }
    }
  ]
}
...
```
> Nótese que la sentencia [hash] sirve para nombrar los archivos en el compilado con un hash seguro en lugar de un nombre que pudiera generar conflictos, por su parte [ext] hace referencia a que se respete su extensión.

Posteriormente agregaremos nuestros archivos binarios dentro de una carpeta en `src/assets/static/`. La forma de hacer referencia a ellos sería, por ejemplo en el header:
```jsx
...
import logo from '../assets/static/image1.png'
import userIcon from '../assets/static/image2.png'

const Header = () => (
  <header className="header">
    <img className="header__img" src={logo} alt="My Netflix" />
    <div className="header__menu">
      <div className="header__menu--profile">
        <img src={userIcon} alt="" />
...
```

## Recibiendo propiedades en un componente
Para recibier propiedades deesde el exterior a un componente, usaremos el caso de customizar el título de cada una de nuestras categorías, tendríamos que agregar un elemento más en la destructuración del componente de la siguiente manera:
```jsx
...
const Categories = ({ children, title }) => (
  <div className="categories">
    <h3 className="categories__title">{title}</h3>
    {children}
  </div>
)
...
```

Luego cuando implementemos este componente, podemos agregarle este valor como si fuera un atributo:
```jsx
/// App.jsx
...
  <Categories title="Peliculas">
    <Carousel>
      <CarouselItem />
    </Carousel>
  </Categories>
...
```

## Añadiendo fonts a nuestro proyecto
Con la finalidad de agregar una fuente, vamos a generar un archivo de *variables* en sass. Primeramente añadimos un archivo llamado `Vars.scss` dentro de `assets/styles`, mismo en el que poremos crear variables que podemos luego importar en cualquier otro archivo de estilos:
```scss
// Vars.scss
@import url(https://fonts.googleapis.com/css?family=Muli&display=swap);
$theme-font: 'Muli', sans-serif;
$main-color: #8f57fd;
```

Para hacer uso de estas variables en otro archivo usamos por ejemplo:
```scss
// App.scss
@import './Vars.scss';

body {
  margin: 0;
  font-family: $theme-font;
  background: $main-color;
}
```

## Añadiendo media queries
Para añadir media queries a nuestro proyecto, podemos generar un archivo en `assets/styles` llamado `Media` y luego importándolo en `App`, sería para propósitos generales.

## Agregando mocks a nuestro proyecto
Para añadir datos de prueba a nuestro sitio, podemos generar un archivo llamado `mocks.json`. Para servirlo a modo de API, instalamos el siguiente paquete:
```bash
npm install -g json-server
```

Una vez instalado, abrimos otra terminal para servir nuestor json con el comando:
```bash
json server mocks.json
```

Veremos que este comando va a servir nuestro json en la url http://localhost:3000/[mainKeyOfYourJson]

# React Hooks
Los react hooks, son una implementación que les da estado y ciclo de vida a los componentes stateless. La intención de estos componentes es ahorrar la sintaxis de clase de los componentes stateful así como facilitar la transmisión de información entre componentes que no necesariamente están anidados.

Esta funcionalidad está disponible a partir de la versión 16.8 de React. Esto significa que si se quisiera implementar en proyectos *legacy* podríamos generar muchas incompatibilidades, por lo cuál se recomienda mayormenta para proyectos nuevos.

Para empezar a crear hooks, modifiquemos nuestra `App`, utilizaremos dos funciones nuevas:
* **useState** para manejar el estado del cmponente
* **useEffect** para manejar transmisiones de datos (patrón observer)
```jsx
// App.jsx
import React, { useState, useEffect } from 'react'
...
```

Luego nos aseguramos de que nuestro componente esté configurado con un return explícito, esto es para poder agregar más lógica al componente en lugar de un solo valor de retorno. En el caso de nuestro componente App tendría que tener esta estructura:
```jsx
// App.jsx
import React, { useState, useEffect } from 'react'
...

const App = () => {
  return(
    <div className="App">
      ...
    </div>
  )
}

export default App
```

## Implementando useState
Para implementar `useState`, generamos dos constantes, en este ejemplo:
```jsx
const [ state, setState ] = useState(initialState);
```
Donde:
* `state`: es el nombre que le daremos a la variable que almacena nuestro estado
* `setState`: la que usaremos para actualizar nuestro estado
* `initialState`: datos iniciales en el estado

En el caso de nuestra `App`, haremos la siguiente implementación, con la cuál, ya tendríamos completamente listo nuestro estado para ser usado en el componente:
```jsx
import React, { useState, useEffect } from 'react'
...

const App = () => {
  const [ videos, setVideos ] = useState({ mylist: [], trends: [], originals: [] });

  useEffect(() => {
    fetch('http://localhost:3000/initialState')
      .then(response => response.json())
      .then(data => setVideos(data))
  }, []);

  return(...)
}

export default App
```
Notemos que la función `useEffect` se encargará de implementar la lógica para recuperar la data que queremos atraer hacia nuestro estado. `useEffect`recibe dos parámetros:
1. La función que se ejecutará cuando se monte el componente, adicional puede devolver otra función que se ejecutará cuando el componente se desmonte,
3. Un array donde podemos especificar qué propiedades deben cambiar para que React vuelva a llamar nuestro código. Si el componente actualiza pero estas props no cambian, la función no se ejecutará.

# Custom hooks
React nos permite separar la lógica de un hook en un archivo aparte, para ello crearemos la carpeta `src/hooks/`. Los componentes que usemos para este efecto, deben llevar el prefijo *use* por definición. Trasladamos la lógica de nuestro hook del componente `App` en un archivo que llamaremos `useMocks.js`:
```javascript
import { useState, useEffect } from 'react'

const useMocks = (API) => {
  const [ videos, setVideos ] = useState({ mylist: [], trends: [], originals: [] });

  useEffect(() => {
    fetch(API)
      .then(response => response.json())
      .then(data => setVideos(data))
  }, []);

  return videos
}

export default useMocks
}
```

Nótese que estamos generando un hook agnóstico para recuperar datos de un JSON, con lo cúal podríamos hacer su implementación en cualquier componente de la siguiente forma:
```jsx
// App.jsx
...
import useMocks from '../hooks/useMocks'

const App = () => {
  const mockState = useMocks('http://localhost:3000/initialState');
  return mockState.length === 0 ? <h1>Loading...</h1> : (
    <div className="App">
      uso de mockState
    </div>
  )
}
...
```

# Proptypes
Proptypes es un feature de React, que nos permite verificar de manera dinámica el tipo de dato de las propiedades que les pasamos a nuestros componentes, asimismo podemos definir si son requeridos o no. Para comenzar, instalamos la siguiente dependencia a nuestro proyecto:
```
npm install prop-types
```

Para implementarlo en cuanquiera de nuestros componentes, lo importamos y lo agregamos justo antes de nuestro export de la siguiente forma:
```jsx
import PropTypes from 'prop-types'
...

const MyComponent = ({ paramNumber, paramString, paramBoolean }) => (
  <div className="carousel-item">
    markup that use params...
  </div>
)

MyComponent.propTypes = {
  paramNumber: PropTypes.number,
  paramString: PropTypes.string,
  paramBoolean: PropTypes.boolean
}

export default MyComponent
```

# Complete project
Check my full project [here](https://github.com/uuzii/my-react-netflix)

# React router
Para empezar a utilizar react router, instalamos la siguiente dependencia:
```bash
npm install react-router-dom --save
```

Generamos la siguiente carpeta en nuestro proyecto `src/routes` y dentro de ella un archivo llaamdo `App.js`, que es donde trabajremos las rutas de nuestro proyecto con la siguiente configuración:
```javascript
import React from 'react'
import { BrowserRouter, Route } from 'react-router-dom'
import Home from '../containers/Home'

const App = () => (
  <BrowserRouter>
    <Route exact path="/" component={Home} />
  </BrowserRouter>
)

export default App
```

Donde:
* `BrowserRoute` será el encargado de empujar al viewport nuestras diferentes vistas
* `Route` será cualquier ruta de nuestra página, en este caso Home (Antes container/App)

Configuramos también nuestro `index.js` de la siguiente manera:
```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import Home from './routes/App'

ReactDOM.render(<App />, document.getElementById('app'))
```

## Agregando más rutas (containers)
Para generar más rutas, creamos otro archivo en la carpeta `containers/`, en este caso `Login.jsx`, generamos su su markup y lo agregamos a la par de nuestra primera ruta en `App.js`:
```javascript
import React from 'react'
import { BrowserRouter, Route } from 'react-router-dom'
import Home from '../containers/Home'
import Login from '../containers/Login'

const App = () => (
  <BrowserRouter>
    <Route exact path="/" component={Home} />
    <Route exact path="/login" component={Login} />
  </BrowserRouter>
)

export default App
```

Posteriormente agregamos la siguiente configuración a nuestro archivo de webpack:
```javascript
{
  ...
    devServer: {
      historyApiFallback: true
    }, 
  ...
}
```

Si recompilamos nuestro proyecto, ya podríamos ver la otra vista en la ruta `/login`

## Agregando Switch
`Switch` nos permitirá hacer cambios entre rutas, para agregarlo, generamos otra vista, en este caso su markup estará definido en el container `Register.jsx`. Para agregarlo configurándolo con `Switch`, modificamos nuestro archivo `App.js`:
```javascript
import React from 'react'
import { BrowserRouter, Switch, Route } from 'react-router-dom'
import Home from '../containers/Home'
import Login from '../containers/Login'
import Register from '../containers/Register'


const App = () => (
  <BrowserRouter>
    <Switch>
      <Route exact path="/" component={Home} />
      <Route exact path="/login" component={Login} />
      <Route exact path="/register" component={Register} />
    </Switch>
  </BrowserRouter>
)

export default App
```

Esta configuración nos ayuda a que solo se presente en el viewport la vista que hemos definido en el path del navegador y no se empujen otras rutas.

### Container 404
Si ingresamos erróneamente la URL del navegador, es decir, una ruta que no existe en la aplicación, es de gran utilidad mostrar una vista por default, en esta caso podemos crear un container llamado `NotFound.jsx`:
```jsx
import React from 'react'

const NotFound = () => (
  <React.Fragment>
    <h1>No encontrado</h1>
    <h2>Volver al home</h2>
  </React.Fragment>
)

export default NotFound
```

Nótese que estamos wrappeando nuestro markuo dentro de un componente llamado `React.Fragment` con la finalidad de tener todo dentro de un solo elemento, ya que jsx solo nos permite tener un elemento en nuestro return, una alternativa de esto es usar `<> Content </>`.

> Tomar en cuenta que mandar más de un elemento en el return, rompe la aplicación.

osteriormente añadiremos a nuestro `Switch`de la siguiente forma:
```javascript
const App = () => (
  <BrowserRouter>
    <Switch>
      <Route exact path="/" component={Home} />
      <Route exact path="/login" component={Login} />
      <Route exact path="/register" component={Register} />
      <Route component={NotFound} />
    </Switch>
  </BrowserRouter>
)
```

> Nótese que no estamos agregando ningún atributo a la ruta NotFound para que esta se muestre por defecto si no se resuelve la ruta.

## Generando un Layout
Un Layout, nos ayudará a persistir elementos como el header y el footer a lo largo de toda nuestra aplicación, para ello generamos nuestros componentes que contengan su markup y también un componente llamado `Layout.jsx` y lo configuramos de la siguiente manera:
```jsx
import React from 'react'
import Footer from './Footer'
import Header from './Header'

const Layout = ({ children }) => (
  <div className="App">
    <Header />
    {children}
    <Footer />
  </div>
)

export default Layout
```

Posteriormente en nuestro archivo `App.js` configuramos de la siguiente forma:
```javascript
import React from 'react'
import { BrowserRouter, Switch, Route } from 'react-router-dom'
import Layout from '../components/Layout'
import Home from '../containers/Home'
import Login from '../containers/Login'
import NotFound from '../containers/NotFound'
import Register from '../containers/Register'


const App = () => (
  <BrowserRouter>
    <Layout>
      <Switch>
        <Route exact path="/" component={Home} />
        <Route exact path="/login" component={Login} />
        <Route exact path="/register" component={Register} />
        <Route component={NotFound} />
      </Switch>
    </Layout>
  </BrowserRouter>
)

export default App
```

## Agregando links de navegación
Una vez que tenemos varias vistas configuradas en nuestro proyecto, solo resta configurar links para acceder a ellas. En los componentes que identifiquemos que poseen estos links, agregaremos el componente `Link` y wrapeamos el elemento que hará el link dentro de este:
```jsx
...
import { Link } from 'react-router-dom'
...

  <Link to="/">
    <img className="header__img" src={logo} alt="My Netflix Video" />
  </Link>
 
...
```

> Si generamos una ruta mediante la etiqueta `<a href="/route"></>` también se hará el ruteo, pero veremos un refresh, el uso del componente `Link` nos evita dicho refresh.

# Redux
Es una librería escrita en JavaScript basada en la arquitectura Flux, propuesta por Facebook e inspirada en Elm, un lenguaje funcional. Redux se bas en tres principios fundamentales:
1. Solo hay una fuente de la verdad. Nuestra aplicación solo debe de tener un único Store y es la única fuente de información.
2. El estado es de solo lectura. La única forma de modificar el estado es emitiendo un acción, este objeto describe lo que va a ocurrir.
3. Podemos usar solo funciones puras. Para realizar cambios al estado es necesario utilizar Reducers los cuales son funciones puras que toman el estado anterior, una acción y devuelve un nuevo estado con las modificaciones necesarias.

Redux nos ayudará a manejar el flujo de información de nuestra aplicación. Asimismo, propone una forma de manejar el estado donde podamos controlar cómo vamos a interactuar con otros elementos (llamadas a un API) o interacciones dentro de nuestra aplicación, teniendo en cuenta esto, Redux intenta de predecir las mutaciones que pueda sufrir el estado, creando restricciones de cuando y como pueden ser ejecutadas las actualizaciones en nuestras aplicaciones.

Para empezar a utilizarlo, instalamos la siguiente dependencia de desarrollo:
```bash
npm install redux react-redux --save
```

Dentro de `src/` generamos dos carpetas nuevas: `actions/` y `reducers/`, luego configuramos de esta manera nuestro `index.js`:
```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import App from './routes/App'

// La función render, recibe el componente a mostrar y el nodo en el cuál se va indexar
ReactDOM.render(
  <Provider>
    <App />
  </Provider>,
  document.getElementById('app')
)
```

Donde:
* `Provider`: nos permitirá encapsular todos nuestros componentes para suministrarles el *store* a cada uno de ellos.
* `createStore`: contendrá la lógica que nos ayudará para almacenar el store y distribuirlo en nuestra aplicación.

En el siguiente ejemplo, pondremos un mock como initalState de nuestra aplicación. Para esto, generamos una constante dentro de nuestro archivo `index.js` con toda la data que queremos agregar a este initialState con una configuración parecida a esta:
```javascript
import React from 'react'
import ReactDOM from 'react-dom'
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import App from './routes/App'

const initialState = {
  "user": {},
  "playing":  {},
  "mylist": [],
  "trends": [ ... ]
}

ReactDOM.render(
  <Provider>
    <App />
  </Provider>,
  document.getElementById('app')
)
```

Donde:
* `user` nos permitirá almacenar la información del usuario.
* `playing` el estado de la reproducción
* `mylist` nuestra lista de reproducción
* `trends`: la data acerca de las tendencias

Ahora generamos otra constante para tener una referencia de nuestro store apoyándonos de `createStore` y posteriormente la agregaremos a nuestro `Provider` de la siguiente forma:
```javascript
...
import { Provider } from 'react-redux'
import { createStore } from 'redux'
import  reducer from './reducers'
import App from './routes/App'

const initialState = {
  "user": {},
  "playing":  {},
  "mylist": [],
  "trends": [ ... ]
}

const store = createStore(reducer, initialState);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById('app')
)
```

Generamos nuestro reducer dentro de la carpeta `src/reducers/` en un archivo llamado `index.js`con la siguiente configuración:
```javascript
const reducer = (state, action) => {
  return state;
}

export default reducer
```

Ahora, identificamos cuál es el elemento que se renderiza en primera instancia de nuestra aplicación, en este caso es el container `Home`. De momento, suistituiremos nuestro custom hook por el store que acabamos de generar, esto será posible mediante le uso del método `connect`:
```jsx
...
// importando connect
import { connect } from 'react-redux'
...
// destructurando los elementos que nos arroja el connect para recibirlos como parámetros
const Home = ({ trends, originals }) => {
  return (
    <>
      ... markup que hace uso de la data ...
    </>
  )
}
// mapeando los elementos del store hacia props que se usarán en este componente
const mapStatetoProps = state => {
  return {
    trends: state.trends,
    originals: state.originals
  }
}

export default connect(mapStatetoProps, null)(Home)
```

Tomemos en cuenta lo siguiente:
* No necesitamos extraer toda la data sino solo la que usaremos en esta vista
* `connect` recibe como parámetros las `props` y las `actions`.


Configurando de este modo nuestro proyecto, podremos manejar el flujo de nuestro mock hacia el container `Home`.
