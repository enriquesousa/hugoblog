---
date: 2023-03-14T10:48:43-07:00
title: "PHP Autocarga y Extracción"
menuTitle: "30. Autocarga"
draft: false
weight: 25
---

Está bien, abróchate el cinturón. Este será el episodio más denso hasta ahora, ya que discutimos una variedad de temas relacionados con la organización del proyecto. Abordaremos las raíces de los documentos, las funciones auxiliares, las constantes, la carga automática de PHP y más.

## Que aprenderemos
- Autoloading Classes
- Document Root
- extract()

## Cambiar la raíz del proyecto
Crear una carpeta publica donde almacenaremos `index.html`, `css`, `js`, etc..
Usar la opción:
`-t <docroot>     Specify document root <docroot> for built-in web server`
del servidor de php, en la CLI podemos correr:
`php -S localhost:8888 -t public`
En `index.php` definimos nuestro BASE_PATH 

En public/index.php
```php
<?php 

const BASE_PATH = __DIR__ . '/../';

// var_dump(BASE_PATH);

require BASE_PATH . 'functions.php';
require base_path('display_errors.php');

spl_autoload_register(function ($class){
    // dd($class);
    // dd(base_path($class . '.php'));
    // require base_path("Core/" . $class . '.php');
    require base_path("Core/{$class}.php");
});

require base_path('router.php');
```

En `functions.php`
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

// check si el usuario esta autorizado
function authorize($condition, $status = Response::FORBIDDEN){
    if (! $condition) {
    abort($status); 
    }
}

function base_path($path){
    return BASE_PATH . $path;
}

function view($path, $attributes = []){
    extract($attributes);
    require base_path('views/' . $path);
}
```

Mover `Database.php`, `Response.php`,  `Validator.php`, `router.php`, `functions.php` a una nueva carpeta que se llame `Core`.

## Autocarga
[`spl_autoload_register`](https://www.php.net/manual/en/function.spl-autoload-register.php)
(PHP 5 >= 5.1.0, PHP 7, PHP 8)

spl_autoload_register — Register given function as __autoload() implementation

 `spl_autoload_register(?callable $callback = null, bool $throw = true, bool $prepend = false): bool`

 Register a function with the spl provided __autoload queue. If the queue is not yet activated it will be activated.

If your code has an existing __autoload() function then this function must be explicitly registered on the __autoload queue. This is because spl_autoload_register() will effectively replace the engine cache for the __autoload() function by either spl_autoload() or spl_autoload_call().

If there must be multiple autoload functions, spl_autoload_register() allows for this. It effectively creates a queue of autoload functions, and runs through each of them in the order they are defined. By contrast, __autoload() may only be defined once. 


Son muchos cambios de paths en todos los archivos, mejor dejamos pendiente para la siguiente lección que volvamos a reescribir el código pero ahora con una mejora mas de re organización al introducir el concepto del:
`name-space`

Listo!

