---
date: 2023-03-01T09:19:26-08:00
title: "Crear Router PHP"
menuTitle: "15. Router PHP"
draft: false
weight: 25
---

Creo que tus habilidades PHP ahora han madurado hasta el punto de que está listo para construir un enrutador PHP personalizado desde cero. Esto nos dará la oportunidad de discutir la organización del código, los códigos de respuesta y más.

## Que aprenderemos
- Routers
- Status Code
- Code Organization


## Carpeta especifica
* * *
Crear una carpeta especifica para los controladores
```php
controllers/ 
```
Luego crear un archivo único index.php que se encargara de administrar las rutas, en `index.php`:
```php
<?php 
require 'functions.php';
$uri = $_SERVER['REQUEST_URI'];

if ($uri === '/') {
    require 'controllers/index.php';
} else if ($uri === '/about') {
    require 'controllers/about.php';
} else if ($uri === '/contact') {
    require 'controllers/contact.php';
} 
```
Podemos ver en la variable `$_SERVER['REQUEST_URI']` que ahi es donde vemos el url que el usuario dio, pero que pasa si el usuario pasa un url invalido o que no existe en nuestros controladores, por ejemplo: `http://localhost:8888/about?name:enrique` vemos parámetros de `key:value` pair, que la podemos ver si la imprimimos con:
```php
$uri = $_SERVER['REQUEST_URI'];
dd($uri);
```
lo cual nos da `string(19) "/about?name:enrique"` .

## Parse URI
* * *
Entonces como podemos eliminar el resto de caracteres que no nos interesa, para eso podemos usar una función de PHP.
```php
$uri = $_SERVER['REQUEST_URI'];
 
// $uri2 = parse_url($uri);
// dd($uri2);

// También la podemos escribir asi
dd(parse_url($uri));
```
lo cual nos da como resultado
```
array(2) {
  ["path"]=>
  string(6) "/about"
  ["query"]=>
  string(12) "name:enrique"
}
```
Entonces ya podemos usar:
```php
<?php 
require 'functions.php';

$uri = parse_url($_SERVER['REQUEST_URI'])['path'];

if ($uri === '/') {
    require 'controllers/index.php';
} else if ($uri === '/about') {
    require 'controllers/about.php';
} else if ($uri === '/contact') {
    require 'controllers/contact.php';
}
```
Con esto ya nos aseguramos que aun que en la URL venga como `http://localhost:8888/about?name:enrique` el program nos sigue redirigiendo correctamente a la pagina `/about` conservando n su forma correcta los parámetros en la URL.

## Simplificar con un array asociativo
* * *
Para simplificar un poco mas las condicionales de los if, podemos usar un array key:value para almacenar las rutas y usar otra función de PHP `array_key_exists($key,$array);`, entonces el código queda así:
```php
<?php 
require 'functions.php';
$uri = parse_url($_SERVER['REQUEST_URI'])['path'];
$routes = [
    '/' => 'controllers/index.php',
    '/about' => 'controllers/about.php',
    '/contact' => 'controllers/contact.php'
];
if ( array_key_exists($uri, $routes) ) {
    require $routes[$uri];
} else {
    http_response_code(404);
    echo "Pagina no encontrada!";
    die();
}
```
Lo podemos mejorar aun mas si hacemos una re-dirección a una pagina que se encargue de desplegar el mensaje de pagina no encontrada!
En views/404.php
```php
<?php require('partials/head.php') ?>
<?php require('partials/nav.php') ?>

<main>
    <div class="mx-auto max-w-7xl py-6 sm:px-6 lg:px-8">
        <h1 class="text-2xl font-bold">Pagina NO encontrada!</h1>
        <p class="mt-4">
            <a href="/" class="text-blue underline">Regresar a inicio.</a>
        </p>   
    </div>
</main>

<?php require('partials/footer.php') ?>
```
Y en index.php
```php
if ( array_key_exists($uri, $routes) ) {
    require $routes[$uri];
} else {
    http_response_code(404);
    require 'views/404.php';
    die();
}
```
Todo funciona bien hasta aquí!
Todavía podemos refactor un poco el código, en index.php
```php
<?php 
require 'functions.php';

$uri = parse_url($_SERVER['REQUEST_URI'])['path'];
$routes = [
    '/' => 'controllers/index.php',
    '/about' => 'controllers/about.php',
    '/contact' => 'controllers/contact.php'
];

// función para display 404 page 
function abort($code = 404){
    http_response_code($code);
    require "views/{$code}.php";
    die();
}

if ( array_key_exists($uri, $routes) ) {
    require $routes[$uri];
} else {
    abort();
}
```
Y por ultimo podemos pasar la función a las funciones generales del sistema y el `array_key_exists($uri, $routes)` pasarlo su propia función
```php
<?php 
require 'functions.php';

$uri = parse_url($_SERVER['REQUEST_URI'])['path'];
$routes = [
    '/' => 'controllers/index.php',
    '/about' => 'controllers/about.php',
    '/contact' => 'controllers/contact.php'
];

// función para route to controller
function routeToController($uri, $routes){
    if ( array_key_exists($uri, $routes) ) {
        require $routes[$uri];
    } else {
        abort();
    }
}

// función para display 404 page 
function abort($code = 404){
    http_response_code($code);
    require "views/{$code}.php";
    die();
}

routeToController($uri, $routes);
```
Por ultimo podemos pasar casi todo el código a su propio archivo router.php
```php
<?php 

$uri = parse_url($_SERVER['REQUEST_URI'])['path'];

$routes = [
    '/' => 'controllers/index.php',
    '/about' => 'controllers/about.php',
    '/contact' => 'controllers/contact.php'
];

// función para route to controller
function routeToController($uri, $routes){
    if ( array_key_exists($uri, $routes) ) {
        require $routes[$uri];
    } else {
        abort();
    }
}

// función para display 404 page 
function abort($code = 404){
    http_response_code($code);
    require "views/{$code}.php";
    die();
}

routeToController($uri, $routes);
```
Con esto tenemos un archivo dedicado exclusivamente para manejar las rutas. Y el index.php nos queda asi:
```php
<?php 

require 'functions.php';
require 'router.php';
```
**Listo!**  {{< line_break >}}
Continuar ...






