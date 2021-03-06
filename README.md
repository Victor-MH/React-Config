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



## Sección III - Babel

Usamos babel para Babel hacer que nuestro código JavaScript sea compatible con todos los navegadores. Para esto necesitaremos las siguientes dependencias para desarrollo con la bandera ***-D***:

**dependencias:**

- @babel/core 
- babel-loader 
- @babel/preset-env 
- @babel/preset-react
````
npm i @babel/core babel-loader @babel/preset-env @babel/preset-react -D
````
Ahora creamos nuestro archivo de configuración llamado ***.babelrc*** en la raíz del proyecto. Dentro de este archivo configuramos los presets:

````javascript
{
    "presets": [
        "@babel/preset-env",   // Nos permite compilar todo tipo de js desde ecmascript 5
        "@babel/preset-react"   // Nos ayuda con la sintaxis de react
    ]
}
````


## Sección IV - Webpack

Webpack es una herramienta que nos va a ayudar a preparar nuestro código para enviarlo a producción, conocido como un module bundler.

Los **module bundlers** nos permiten trabajar con javascript, archivos estáticos, imagenes, fuentes entre otros.

Para esto necesitaremos las siguientes dependencias para desarrollo con la bandera ***-D***:

**dependencias:**

- webpack
- webpack-cli 
- webpack-dev-server

````
npm i webpack webpack-cli webpack-dev-server -D
````
Ahora creamos nuestro archivo de configuración llamado ***webpack.config.js*** en la raíz del proyecto. Dentro de este archivo configuramos lo siguiente:

***entry:*** este es el archivo de entrada de nuestro proyecto que como mencionamos antes es ***/src/index.js***

***output:*** aquí configuramos donde queremos obtener el resultado de nuestro bundle, es decir el contenido que enviaremos a producción.

***resolve:*** aquí configuramos las extensiones que tiene nuestro proyecto que al ser de react usaremos **.js** y **.jsx**

***module:*** en este apartado configuramos las "reglas" del proyecto para cada tipo de archivo, iniciamos con **.js** y **.jsx**

Finalmente nuestro archivo de configuración inicial para webpack queda de la siguiente manera:

````js
const path = require('path');

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
                    loader: 'babel-loader'
                }
            }
        ]
    }
}
````


## Sección V - HTML con Webpack

Necesitamos el loader y el plugin para comenzar a trabajar con html y poder probar nuestro proyecto.

Para esto necesitaremos las siguientes dependencias para desarrollo con la bandera ***-D***:

**dependencias:**

- html-loader
- html-webpack-plugin 

````
npm i html-loader html-webpack-plugin -D
````
En nuestro archivo de configuración ***webpack.config.js*** vamos a configurar el loader y el plugin para hacer uso de html.

Primero traemos el plugin creando una constante al inicio del proyecto.
````js
const HtmlWebpackPlugin = require('html-webpack-plugin');
````

Luego creamos nuestra nueva regla para usar el loader.
````js
{
   test: /\.html$/,
   use: {
      loader: 'html-loader'
   }
}
````

Ahora creamos el apartado de ***plugins***, que será un array, al primer nivel de nuestro objeto de configuración *(al nivel de entry, output, resolve y module)*.
````js
plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html',
            filename: './index.html'    
        })
    ]
````

Finalmente para probar nuestra aplicación tenemos que crear los siguiente scripts en nuestro archivo ***package.json***
````js
"scripts": {
   "dev": "webpack serve --mode development",
   "build": "webpack --mode production"
},
````

Con estos scripts podemos probar el primero y así iniciar nuestro servidor de desarrollo.



## Sección VI - Estilos CSS y SASS con Webpack

Algo que no puede faltar en nuestros proyectos son los estilos, para estos usaremos sass que nos facilitará trabajar con css dentro de nuestro proyecto.

Para esto necesitaremos las siguientes dependencias para desarrollo con la bandera ***-D***:

**dependencias:**

- mini-css-extract-plugin
- css-loader
- sass
- sass-loader 

````
npm i mini-css-extract-plugin css-loader sass sass-loader -D
````
En nuestro archivo de configuración ***webpack.config.js*** vamos a configurar los loaders y plugins para hacer uso de sass.

Primero traemos el plugin creando una constante al inicio del proyecto.
````js
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
````

Luego creamos nuestra nueva regla para usar el loader.
````js
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
````

Ahora creamos el apartado de ***plugins***, que será un array, al primer nivel de nuestro objeto de configuración *(al nivel de entry, output, resolve y module)*.
Anexando este plugin al de html que instanciamos anteriormente.
````js
plugins: [
        new MiniCssExtractPlugin({
            filename: '[name].css'
        })
    ]
````

Ahora pasamos a crear la estructura de carpetas donde incluiremos los estilos de nuestro proyecto.
- /src/styles
- /src/styles/components

También creamos un archivo ***globals.scss*** dentro de **/src/styles*** para probar nuestra app y donde incluiremos los estilos globales de la aplicación.
````css
$base-color: #C6538C;
$color: rgba(black, 0.88);

body {
    background-color: $base-color;
    color: $color;
}
````

Es importante **importar** este archivo en nuestro ***index.js*** para que se utilicen los estilos que creamos. Esto lo hacemos de la siguiente manera:
````js
import './styles/global.scss';
````



## Sección VII - Optimización de Webpack

Casi tenemos listo nuestro proyecto, aún podemos optimizar nuestra configuración para tenerlo listo al momento de generar el build para producción.

Para esto necesitaremos las siguientes dependencias para desarrollo con la bandera ***-D***:

**dependencias:**

- css-minimizer-webpack-plugin
- terser-webpack-plugin
- clean-webpack-plugin
````
npm i css-minimizer-webpack-plugin terser-webpack-plugin clean-webpack-plugin -D
````
Esta vez vamos a crear un nuevo archivo de configuración para webpack ***webpack.config.dev.js*** de esta forma separamos las configuraciones para desarrollo y las de producción.

En el archivo de configuración para desarrollo vamos a copiar todo  lo que ya tenemos en nuestro primer archivo de configuración, *sin eliminar el otro archivo*, y agregaremos lo siguiente:

````js
resolve: {...},
mode: 'development',
module: {...},
...
//al final, después de *plugins*
devServer: {
   contentBase: path.join(__dirname, 'dist'),
   compress: true,
   port: 8080
}
````
Ahora en el archivo de configuración para producción traemos los paquetes que instalamos antes:
````js
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');
const TerserPlugin = require('terser-webpack-plugin');
const { cleanWebpackPlugin } = require('clean-webpack-plugin');
````
También agregamos el parámetro **mode** de la siguiente manera:
````js
resolve: {...},
mode: 'production',
module: {...},
````

En el objeto ***output*** agregamos el parámetro **publicPath**
````js
output: {
   path: path.resolve(__dirname, 'dist'),
   filename: 'bundle.js',
   publicPath: '/',
}
````

También vamos a modificar el objeto ***resolve*** de la siguiente manera:
````js
resolve: {
   extensions: ['.js', '.jsx'],
   alias: {
      '@components': path.resolve(__dirname, 'src/components/'),
      '@styles': path.resolve(__dirname, 'src/styles')
   }
}
````

En la sección de plugins agregamos **CleanWebpackPlugin**
````js
plugins: [
   new HtmlWebpackPlugin({
      template: './public/index.html',
      filename: './index.html'    
   }),
   new MiniCssExtractPlugin({
      filename: '[name].css'
   }),
   new CleanWebpackPlugin(),
]
````

Finalmente agregamos el parámetro **optimization** después de **plugins** que será el siguiente objeto:
````js
optimization: {
   minimize: true,
   minimizer: [
      new CssMinimizerPlugin(),
      new TerserPlugin(),
   ]
}
````

Por último vamos a modificar los scripts que creamos en ***package.json*** antes para que utilicemos los archivos de configuración correspondientes a cada modo.
````js
"scripts": {
   "dev": "webpack serve --config webpack.config.dev.js",
   "build": "webpack --config webpack.config.js"
},
````