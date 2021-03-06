# Estructura de Proyecto avanzado con Gulp

----------------------------------------
## ➥ Comandos:
----------------------------------------

### Instalación únicas

```shell
npm install --global gulp-cli
```

### Instalación que se hace en cada proyecto en el que se quiera usar gulp.

```shell
npm init
```

```shell
npm install --save-dev gulp
```

### Crear un archivo (gulpfile.js) en el proyecto

----------------------------------------
## ➥ Plugins:
----------------------------------------

### ::pug::

Complemento Gulp para compilar plantillas Pug. Permitiéndole compilar sus plantillas Pug en HTML o JS, con soporte para plantillas locales, filtros Pug       personalizados, envoltura AMD y otros.

```shell
npm install --save-dev gulp-pug
```
### ::Sass::

Una implementación JavaScript pura de Sass .

● Link: https://www.npmjs.com/search?q=sass

```shell
npm i sass gulp-sass --save-dev
```
### ::minifier::

Minify HTML, JS, CSS with html-minifier, terser, clean-css.

● Link: https://www.npmjs.com/package/gulp-minifier

```shell
npm i --save-dev gulp-minifier
```

### ::postcss::

PostCSS es una herramienta para transformar estilos con complementos JS. Estos complementos pueden filtrar su CSS, admitir variables y mixins, transpilar la sintaxis CSS futura, imágenes en línea y más.

● Link: https://www.npmjs.com/package/postcss

```shell
npm i --save-dev gulp-postcss
```
### ::cssnano::

cssnano toma su CSS bien formateado y lo ejecuta a través de muchas optimizaciones enfocadas, para garantizar que el resultado final sea lo más pequeño posible para un entorno de producción.

● Link: https://www.npmjs.com/package/cssnano

```shell
npm i --save-dev gulp-cssnano 
```
### ::browsersync::

```shell
npm install browser-sync --save-dev
```
### ::imagemin::

Optimizador de Imagenes, de todo tipo con sus plugins para cada tipo de imagen ()

● Link: https://www.npmjs.com/package/gulp-imagemin

```shell
npm i --save-dev gulp-imagemin(no funciona)
```

```shell
npm i --save-dev gulp-imagemin@7.0.0 (recomendado)
```

```shell
npm i --save-dev imagemin-pngquant
```

----------------------------------------
## ➥ Codigo gulpfile.js:
----------------------------------------
```shell
/* Importar */
const { src, dest, watch, series } = require('gulp');
const sass = require('gulp-sass')(require('sass'));
const pug = require('gulp-pug');
const minify = require('gulp-minifier');
const postcss = require('gulp-postcss');
const cssnano = require('cssnano');
const imagemin = require('gulp-imagemin') 
const imageminPngquant = require('imagemin-pngquant');
const browsersync = require('browser-sync').create();

// Sass
function scssTask(){
  return src('src/scss/style.scss',{sourcemaps: true})
  .pipe(sass({outputStyle: 'compressed'}).on('error', sass.logError))
  .pipe(postcss([cssnano()]))
  .pipe(dest('public/css',{sourcemaps:'.'}));
}

// Pug
function pugTask(){
  return src('src/views/**/*.pug')
  .pipe(pug())
  .pipe(dest('public/'));
}

//Minify
function minifyTask(){
  return src('src/views/*.html', { allowEmpty: true }) 
  .pipe(minify(
  {minify: true,
  minifyHTML: {
  collapseWhitespace: true,
  conservativeCollapse: true,}}
  ))
  .pipe(dest('public/'))
}

// Browsersyn
function browsersyncServe(cb){
  browsersync.init({
  server:{
  baseDir: 'public/'
  }
  });
  cb();
}

function browsersyncReload(cb){
  browsersync.reload()
  cb();  
}

// Imagemin
function imageminOptimized (){
  return src('src/assets/**/*')
  .pipe(imagemin([imageminPngquant({quality: [0.3, 0.5]})]))
  .pipe(dest('public/assets/'))
}

function watchTask(){
  watch('public/*.html' , browsersyncReload);
  watch('src/views/**/*.pug' , series(pugTask, browsersyncReload));
  watch(['src/scss/**/*.scss'], series(scssTask,browsersyncReload));
}

exports.minihtml = minifyTask;

exports.image = imageminOptimized;

exports.default = series(
  scssTask,
  pugTask,
  browsersyncServe,
  browsersyncReload, 
  imageminOptimized,
  watchTask
);

```

➤ https://cosasdedevs.com/posts/automatizar-tareas-gulpjs/

----------------------------------------
# Estructura de Carpetas de SASS o SCSS
----------------------------------------

![alt text](https://i.ibb.co/syJTKMk/Ficheros-SCSS.png)


