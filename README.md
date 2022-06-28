----------------------------------------
# Estructura de Proyecto avanzado con Gulp
----------------------------------------
➥ Comandos:

➥ Instalación únicas
    1. npm install --global gulp-cli
----------------------------------------
➥ Instalación que se hace en cada proyecto en el que se quiera usar gulp.
    1. npm init
    2. npm install --save-dev gulp
----------------------------------------
➥ Crear un archivo (gulpfile.js) en el proyecto
----------------------------------------
➥ Plugins:
----------------------------------------

::pug::

  Complemento Gulp para compilar plantillas Pug. Permitiéndole compilar sus plantillas Pug en HTML o JS, con soporte para plantillas locales, filtros Pug personalizados, envoltura AMD y otros.

  ➤ npm install --save-dev gulp-pug
  
::Sass::

  Una implementación JavaScript pura de Sass .

  ●Link: https://www.npmjs.com/search?q=sass

  ➤ npm i sass gulp-sass --save-dev

::postcss::

  PostCSS es una herramienta para transformar estilos con complementos JS. Estos complementos pueden filtrar su CSS, admitir variables y mixins, transpilar la sintaxis CSS futura, imágenes en línea y más.

  ● Link: https://www.npmjs.com/package/postcss

  ➤ npm i --save-dev gulp-postcss

::cssnano::

  cssnano toma su CSS bien formateado y lo ejecuta a través de muchas optimizaciones enfocadas, para garantizar que el resultado final sea lo más pequeño posible para un entorno de producción.

  ● Link: https://www.npmjs.com/package/cssnano

  ➤ npm i --save-dev gulp-cssnano 

::browsersync::

  ➤ npm i --save-dev browsersync

::imagemin::

  Oprimizador de Imagenes, de todo tipo con sus plugins para cada tipo de imagen ()

  ● Link: https://www.npmjs.com/package/gulp-imagemin

  ➤ npm i --save-dev gulp-imagemin(no funciona)

  ➤ npm i --save-dev gulp-imagemin@7.0.0 (recomendado)

  ➤ npm i --save-dev imagemin-pngquant
  
----------------------------------------
➥ Codigo gulpfile.js:
----------------------------------------

/* Importar */
const { src, dest, watch, series } = require('gulp');
const sass = require('gulp-sass')(require('sass'));
const pug = require('gulp-pug'); 
const postcss = require('gulp-postcss');
const cssnano = require('cssnano');
const imagemin = require('gulp-imagemin') 
const imageminPngquant = require('imagemin-pngquant');
const browsersync = require('browser-sync').create();



/* Task */

//Sass
function scssTask(){
  return src('dev/scss/style.scss',{sourcemaps: true})
    .pipe(sass())
    .pipe(postcss([cssnano()]))
    .pipe(dest('public/css',{sourcemaps:'.'}));
}

//Pug
function pugTask(){
  return src('dev/views/**/*.pug')
    .pipe(pug())
    .pipe(dest('public/'));
}

//Browsersyn
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

//Imagemin
function imageminOptimized (){
  return src('dev/assets/**/*')
    .pipe(imagemin([imageminPngquant({quality: [0.3, 0.5]})]))
    .pipe(dest('public/assets/'))
}


function watchTask(){
  watch('dev/assets/**/*' ,series(imageminOptimized, browsersyncReload));
  watch('public/*.html' , browsersyncReload);
  watch('dev/views/**/*.pug' ,series(pugTask, browsersyncReload));
  watch(['dev/scss/**/*.scss'],series(scssTask,browsersyncReload));
}

//Task gulp default

exports.default = series(
  scssTask,
  pugTask,
  browsersyncServe,
  browsersyncReload, 
  imageminOptimized,
  watchTask
);



