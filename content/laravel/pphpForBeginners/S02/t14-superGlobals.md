---
date: 2023-03-01T09:19:13-08:00
title: "Super-globales y estilo de página actual"
menuTitle: "14. Super Globales"
draft: false
weight: 20
---

A menudo, deberá aplicar estilos o activar cierta lógica en función de la página actual. Afortunadamente, podemos usar la matriz super-global $_SERVER de PHP para determinar en forma dinámica la página actual.

Vamos a resolver el problema que nos quedo de que no se veía la selección del tema en el bar nav.

## var_dump()
Si colocamos un var_dump() en index.php:
```php
<?php
$heading = 'Home';
// variable dump
var_dump(['Hola']);
require "views/index.view.php";
```
Vemos que en la pagina home nos da ```array(1) { [0]=> string(4) "Hola" } ``` con esto vemos que podemos pasar objetos a la vista. PHP incluye un termina que se llama **Super Global** que son variables que son accesibles de cualquier lado (archivo). Y las podemos usar para tomar información de un **GET_request**, o un **POST_request**, y también podemos pedir información del **_SERVER**.
Si lo mandamos asi:
```php
var_dump($_SERVER); 
```
Obtenemos:
```php
array(26) { ["DOCUMENT_ROOT"]=> string(61) "/home/enrique/laravel/EnriqueSousa/learnPHP/beginnersLaracast" ["REMOTE_ADDR"]=> string(9) "127.0.0.1" ["REMOTE_PORT"]=> string(5) "60438" ["SERVER_SOFTWARE"]=> string(28) "PHP 8.2.0 Development Server" ["SERVER_PROTOCOL"]=> string(8) "HTTP/1.1" ["SERVER_NAME"]=> string(9) "localhost" ["SERVER_PORT"]=> string(4) "8888" ["REQUEST_URI"]=> string(1) "/" ["REQUEST_METHOD"]=> string(3) "GET" ["SCRIPT_NAME"]=> string(10) "/index.php" ["SCRIPT_FILENAME"]=> string(71) "/home/enrique/laravel/EnriqueSousa/learnPHP/beginnersLaracast/index.php" ["PHP_SELF"]=> string(10) "/index.php" ["HTTP_HOST"]=> string(14) "localhost:8888" ["HTTP_USER_AGENT"]=> string(78) "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/110.0" ["HTTP_ACCEPT"]=> string(85) "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8" ["HTTP_ACCEPT_LANGUAGE"]=> string(14) "en-US,en;q=0.5" ["HTTP_ACCEPT_ENCODING"]=> string(17) "gzip, deflate, br" ["HTTP_REFERER"]=> string(31) "http://localhost:8888/about.php" ["HTTP_CONNECTION"]=> string(10) "keep-alive" ["HTTP_UPGRADE_INSECURE_REQUESTS"]=> string(1) "1" ["HTTP_SEC_FETCH_DEST"]=> string(8) "document" ["HTTP_SEC_FETCH_MODE"]=> string(8) "navigate" ["HTTP_SEC_FETCH_SITE"]=> string(11) "same-origin" ["HTTP_SEC_GPC"]=> string(1) "1" ["REQUEST_TIME_FLOAT"]=> float(1677541030.1457650661468505859375) ["REQUEST_TIME"]=> int(1677541030) }  
```
Pero si lo envolvemos en un pre tag.
```php
echo "<pre>";
var_dump($_SERVER);
echo "</pre>"; 
```
El resultado ya es algo como:
<details>
  <summary>Click to see code. </summary>

```php
array(26) {
  ["DOCUMENT_ROOT"]=>
  string(61) "/home/enrique/laravel/EnriqueSousa/learnPHP/beginnersLaracast"
  ["REMOTE_ADDR"]=>
  string(9) "127.0.0.1"
  ["REMOTE_PORT"]=>
  string(5) "45150"
  ["SERVER_SOFTWARE"]=>
  string(28) "PHP 8.2.0 Development Server"
  ["SERVER_PROTOCOL"]=>
  string(8) "HTTP/1.1"
  ["SERVER_NAME"]=>
  string(9) "localhost"
  ["SERVER_PORT"]=>
  string(4) "8888"
  ["REQUEST_URI"]=>
  string(1) "/"
  ["REQUEST_METHOD"]=>
  string(3) "GET"
  ["SCRIPT_NAME"]=>
  string(10) "/index.php"
  ["SCRIPT_FILENAME"]=>
  string(71) "/home/enrique/laravel/EnriqueSousa/learnPHP/beginnersLaracast/index.php"
  ["PHP_SELF"]=>
  string(10) "/index.php"
  ["HTTP_HOST"]=>
  string(14) "localhost:8888"
  ["HTTP_USER_AGENT"]=>
  string(78) "Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/110.0"
  ["HTTP_ACCEPT"]=>
  string(85) "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8"
  ["HTTP_ACCEPT_LANGUAGE"]=>
  string(14) "en-US,en;q=0.5"
  ["HTTP_ACCEPT_ENCODING"]=>
  string(17) "gzip, deflate, br"
  ["HTTP_REFERER"]=>
  string(31) "http://localhost:8888/about.php"
  ["HTTP_CONNECTION"]=>
  string(10) "keep-alive"
  ["HTTP_UPGRADE_INSECURE_REQUESTS"]=>
  string(1) "1"
  ["HTTP_SEC_FETCH_DEST"]=>
  string(8) "document"
  ["HTTP_SEC_FETCH_MODE"]=>
  string(8) "navigate"
  ["HTTP_SEC_FETCH_SITE"]=>
  string(11) "same-origin"
  ["HTTP_SEC_GPC"]=>
  string(1) "1"
  ["REQUEST_TIME_FLOAT"]=>
  float(1677541212.8150699138641357421875)
  ["REQUEST_TIME"]=>
  int(1677541212)
} 

```
</details>

Podemos usar la función die(); para que no sig ejecutando código.
```php
echo "<pre>";
var_dump($_SERVER);
echo "</pre>";
die(); 
```
Podemos hacer una función que se llame dump and die **dd()** para incluir todo este código.
```php
function dd($value) {
    echo "<pre>";
    var_dump($value);
    echo "</pre>"; 
    die(); 
}

dd($_SERVER); 
```
Si queremos saber que tiene la variable $heading
```php
<?php
$heading = 'Home';

function dd($value) {
    echo "<pre>";
    var_dump($value);
    echo "</pre>"; 
    die(); 
}

dd($heading); 
```
Ahora si inspeccionamos 
```php
dd($_SERVER); 
```  
y vemos:
```php
["REQUEST_URI"]=>
  string(1) "/" 
```
Vemos que siempre nos redirecciona a home, asi que podemos inspeccionar la variable "REQUEST_URI" para saber cual es la pagina actual.
```php
echo $_SERVER['REQUEST_URI']; 
```
Con esto ya podemos saber cual es la pagina actual.

Ahora vamos al views/partials/nav.php para el home page
```php
<a href="/" class="<?php if ($_SERVER['REQUEST_URI'] === "/") { echo 'bg-gray-900 text-white'; } else { echo 'text-gray-300'; } ?>  hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium" aria-current="page">Home</a> 
```
Esto funciona! pero hay una forma mejor para evitar tanto código dentro de la clase, tomamos la lógica.
```php
if ($_SERVER['REQUEST_URI'] === "/") {
    echo 'bg-gray-900 text-white';
} else {
    echo 'text-gray-300';
} 
```
Esto mismo lo podemos hacer con lógica ternaria de php asi:
```php
echo $_SERVER['REQUEST_URI'] === "/" ? 'bg-gray-900 text-white' : 'text-gray-300'; 
```

## nav.php
Entonces ahora en nav.php
```php
<a href="/" class="<?php echo $_SERVER['REQUEST_URI'] === "/" ? 'bg-blue-900 text-white' : 'text-gray-300'; ?>  hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium" aria-current="page">Home</a> 
```
Podemos simplificar un poco mas con '<?=' para quiter el echo y quitando el ';' asi
```php
<a href="/" class="<?= $_SERVER['REQUEST_URI'] === "/" ? 'bg-blue-900 text-white' : 'text-gray-300' ?>  hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium" aria-current="page">Home</a> 
```
los tres links quedan asi
```php
<a href="/" class="<?= $_SERVER['REQUEST_URI'] === "/" ? 'bg-blue-900 text-white' : 'text-gray-300' ?>  hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium" aria-current="page">Home</a>
<a href="/about.php" class="<?= $_SERVER['REQUEST_URI'] === "/about.php" ? 'bg-blue-900 text-white' : 'text-gray-300' ?> hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium">Nosotros</a>
<a href="/contact.php" class="<?= $_SERVER['REQUEST_URI'] === "/contact.php" ? 'bg-blue-900 text-white' : 'text-gray-300' ?> hover:text-white px-3 py-2 rounded-md text-sm font-medium">Contactarnos</a> 
```
Ya lo limpiamos un poco.

## index.php
Pero aun lo podemos limpiar mas!
si colocamos esta función en index.php
```php
// Me regresa true or false
function urlIs($value){
    return $_SERVER['REQUEST_URI'] === $value;
} 
```

## nav.php
y sustituimos en nav.php $_SERVER['REQUEST_URI'] === "/"
por la función nos queda:
```php
<?= urlIs("/") ? 'bg-blue-900 text-white' : 'text-gray-300' ?> 
```
Y aunque parezca una mejora muy pequeña, nos ayuda mucho!
Ahora esto solo funciona para el home page, si queremos que funcione para todas las paginas, tenemos que extraer las funciones que queramos que puedan ver todas nuestras paginas.

## functions.php
Entonces creamos en la raíz del proyecto functions.php
```php
<?php 

// función Dump and Die
function dd($value) {
    echo "<pre>";
    var_dump($value);
    echo "</pre>"; 
    die(); 
}

// Me regresa true or false
function urlIs($value){
    return $_SERVER['REQUEST_URI'] === $value;
}
```
Y por lo pronto si tenemos que requerir el archivo para cada una de las paginas:

## index.php
```php
<?php 
require 'functions.php';
$heading = 'Home';
require "views/index.view.php"; 
```

## about.php
```php
<?php 
require 'functions.php';
$heading = 'About Us';
require "views/about.view.php"; 
```

## contact.php
```php
<?php 
require 'functions.php';
$heading = 'Contact Us';
require "views/contact.view.php"; 
```

## nav.php
Y los links de navegación en nav.php
```php
<a href="/" class="<?= urlIs("/") ? 'bg-blue-900 text-white' : 'text-gray-300' ?>  hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium" aria-current="page">Home</a>
<a href="/about.php" class="<?= urlIs("/about.php") ? 'bg-blue-900 text-white' : 'text-gray-300' ?> hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium">Nosotros</a>
<a href="/contact.php" class="<?= urlIs("/contact.php") ? 'bg-blue-900 text-white' : 'text-gray-300' ?> hover:text-white px-3 py-2 rounded-md text-sm font-medium">Contactarnos</a> 
```
**Listo!**  {{< line_break >}}
Ya tenemos navegación dinámica para cada pagina!

