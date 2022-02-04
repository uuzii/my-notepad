# Publicar en NPM
Primeramente tenemos que reestructurar nuestros proyectos, tendríamos que generar dos carpetas en la base de nuestro proyecto:
```bash
| website/
| project/
---| src/
```
Dentro de website pondremos nuestro punto de acceso, los assets y los archivos generales del proyecto como el package, el sw y el gitignore. Dentro de project (que llevará el nombre de nuestro proyecto) colocaremos el código fuente.

Luego nos posicionamos en nuestro proyecto e inicializamos npm:
```bash
cd project
npm init -y
```

Instalamos typescript, ya que nuestro código debe estar en JavaScript:
```bash
npm i -D typescript
```

Agregamos nuestro nombre y el comando build en el package, que nos servirá para pasar nuestro código a js en la carpeta lib:
```json
{
  "name": "@uuzii/platzimediaplayer",
  "version": "1.0.0",
  "description": "",
  "main": "lib index.js",
  "scripts": {
    "build": "tsc src/**/*.ts --outDir lib",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

Para generar nuestor código en javascript usamos el script build:
```bash
npm run build
```

En nuestro package, configuramos como main el archivo principal pero ahora el de lib, ejemplo:
```json
{
  ...
  "main": "lib/MainClass.js"
  ...
}
````

Agregamos un gitignore:
```gitignore
node_modules
lib
```