# Metodologías de CSS
Nos pueden ayudar a mantener mejor organizado nuestro código y hacerlo más escalable

## OOCSS (Object Oriented CCS)
Separa el diseño del contenido, ejemplo: generer una clase global que maneje el width de 100% y luego ponerla a todos los elementos que la necesiten.

## BEM (Block Element Modifier)
Separa los elementos por bloques, elemento y modificador con la siguiente nomenclatura:
  > block__element--modifier

## SMACCS (Scalable & Modular Architecture of CSS)
Consiste en 5 pasos, que consisten en dividir todos los elementos en las siguientes clasificaciones:
1. **Base.** Son los que se usan en toda la aplicación, ejemplo: los botones
2. **Layout.** Son los que se utilizan en la página una sola vez, ejemplo: header o footer
3. **Module.** Son elementos que se repiten una o dos veces
4. **State.** Representa el estado de los elementos que pueden cambiar
5. **Theme.** Se refiere a los modificadores que solo alterarán colores o fuentes en la aplicación

## ITCSS (Inverted Triangle CSS)
Indica que debemos dividir todos nuestros archivos de css para que no se confindan entre sí, la prioridad, de menor a mayor especificidad es:
1. Ajustes
2. Herramientas
3. Genérico
5. Elementos
6. Objetos
7. Componentes
8. Utilidades

## Atomic design
Separar los elementos en un modelo biológico de:
1. Átomos
2. Moléculas
3. Organismos
4. Templates
5. Páginas

# Preprocesadores
Los archivos de CSS eventualmente se pueden hacer muy extensos, para reducir estos archivos así como la escritura de los mismos, nos pueden ayudar los preprocesadores, mismos que tiene una sintaxis expecífica de uso y compilarán nuestro código. Los más usados son:
* [LESS](https://sass-lang.com/guide)
* [SASS](http://lesscss.org/)
* [Stylus](http://stylus-lang.com/)

## SASS
Para empezar a usar sass, tenemos que instalarlo mediante el comando:
  `npm install -g sass``

### Sintaxis de sass
* **Variables**. Para declararlas, escribimos directamente en el archivo `$var: valor`. Para usarlas, colocamos `property: $var`
* **Anidamiento**. Podemos incluir selectores dentro de otros, ejemplo:
  ```css
  .header {
    background: gray,
    h1 {
      color: blue
    }
  }
  ```
* **Herencia**. Podemos heredar las propiedades de una clase a otros selectores utilizando `@extend .className` dentro de un selector.
* **Mixins**. Funcionan parecido a la herencia, pero se denfinen de la siguiente manera: `@mixin mixinName { }` y para usarlas en otro selector colocamos `@include mixinName`

### Para compilar sass en css
Debemos tener un archivo extensión scss con su sintaxis apropiada.

Para compilarlo en un archivo css que pueda entender el navegador, usaremos el comando:
  > `sass [ruta/absoluta/al/archivo/sass] [ruta/absoluta/al/archivo/destino.css]`

# Storybook
Es una guía donde podemos documentar cada componente de nuestra aplicación: qué componentes hay, su diseño y comportamientos.
> [Documentación](https://storybook.js.org/docs/html/get-started/introduction)

