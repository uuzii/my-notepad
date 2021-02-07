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

