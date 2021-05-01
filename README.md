# Entorno de desarrollo para React.js

## Sección I - Inicialización
Inicializamos un repositorio de ***git*** y un paquete con ***npm***.
También creamos los primeros archivos y estructura de carpetas.

- npm init -y
- git init
- Crear carpetas
   1. /src
   2. /public
   3. /src/components 
   4. /src/containers
- Crear archivos
   1. /src/index.js
   2. /public/index.html


## Sección II - Inicialización 2 
### React

Ahora instalamos lo básico para utilizar react, con la bandera ***-S*** para que se guarden como dependencias, no dependencias de desarrollo
````bash
npm i react react-dom -S
````

Editamos el archivo /src/index.js que será el punto de entrada para nuestro proyecto

```javascript
import React from 'react';
import ReactDom from "react-dom";

ReactDOM.render(
   <h1>Hello World<h1/>,
   document.getElementById('app')
);
```
En nuestro archivo /public/index.html solo hay que agregar un ***<div/>*** con el mismo id que usamos en ***/src/index.js***, en este caso ***'app'***.

Para no subir la carpeta ***node_modules/*** a nuestro repositorio de git *(cosa que nunca debemos hacer)* creamos un archivo ***.gitignore*** en la raíz del proyecto.
Podemos utilizar el siguiente [***template***](https://gist.github.com/gndx/747a8913d12e96ff8374e2125efde544).
