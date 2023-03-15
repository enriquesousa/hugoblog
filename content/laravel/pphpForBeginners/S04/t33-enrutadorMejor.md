---
date: 2023-03-15T15:26:37-07:00
title: "Construya un mejor enrutador"
menuTitle: "33. Enrutador"
draft: false
weight: 40
---

En este episodio, construiremos un enrutador mejor que pueda manejar y responder a cualquier tipo de solicitud de formulario. Sin embargo, debido a que los formularios solo admiten `GET` y `POST` de forma nativa, necesitaremos usar un campo de entrada oculto para falsificar el tipo de solicitud.

## Que aprenderemos
- Request Methods
- Request Type Spoofing
- Routing

## Cambiar nombre a Core/router.php a `Core/Router.php`
Ya que vamos a crear una clase en este archivo!
Tenemos que preparar los métodos que vamos a poder manejar así:
```php
<?php 

class Router {
    public function get(){

    }

    public function post(){
        
    }

    public function delete(){
        
    }

    public function patch(){
        
    }

    public function put(){
        
    }
}
```

## Código en  `Core/Router.php`
```php
<?php 

namespace Core;

class Router {
    protected $routes = [];

    public function add($method, $uri, $controller){
        $this->routes[] = compact('method', 'uri', 'controller');
    }

    public function get($uri, $controller){
        $this->add('GET', $uri, $controller);
    }

    public function post($uri, $controller){
        $this->add('POST', $uri, $controller);
    }

    public function delete($uri, $controller){
        $this->add('DELETE', $uri, $controller);
    }

    public function patch($uri, $controller){
        $this->add('PATCH', $uri, $controller);
    }

    public function put($uri, $controller){
        $this->add('PUT', $uri, $controller);
    }


    public function route($uri, $method){

        foreach ($this->routes as $route) {
            if ($route['uri'] === $uri && $route['method'] === strtoupper($method) ) {
                return require base_path($route['controller']);
            }
        }

        $this->abort();
    }

    protected function abort($code = 404){
        http_response_code($code);
        require base_path("views/{$code}.php");
        die();
    }
}
```

## Código en  `public/index.php`
```php
<?php 
const BASE_PATH = __DIR__ . '/../';

require BASE_PATH . 'Core/functions.php';
require base_path('Core/display_errors.php');

spl_autoload_register(function ($class){
    // dd($class);
    // dd(base_path($class . '.php'));
    // require base_path("Core/" . $class . '.php');
    $class = str_replace('\\', DIRECTORY_SEPARATOR, $class);
    // $result = "{$class}.php";
    // dd($result);
    require base_path("{$class}.php");
});

$router = new \Core\Router();
$routes = require base_path('routes.php');
$uri = parse_url($_SERVER['REQUEST_URI'])['path'];
$method = $_POST['_method'] ?? $_SERVER['REQUEST_METHOD'];

$router->route($uri, $method);
```

## Código en  `routes.php`
```php
<?php 

$router->get('/', 'controllers/index.php');
$router->get('/about', 'controllers/about.php');
$router->get('/contact', 'controllers/contact.php');

$router->get('/notes', 'controllers/notes/index.php');
$router->get('/note', 'controllers/notes/show.php');
$router->get('/notes/create', 'controllers/notes/create.php');
$router->delete('/note', 'controllers/notes/destroy.php');
```

## Código en  `views/notes/show.view.php`
```php
<!-- html head -->
<?php require base_path('views/partials/head.php') ?>
<!-- Navegación -->
<?php require base_path('views/partials/nav.php') ?>
<!-- Encabezado (Banner)-->
<?php require base_path('views/partials/banner.php') ?>

<!-- Contenido -->
<main>
    <div class="mx-auto max-w-7xl py-6 sm:px-6 lg:px-8">

        <p class="mb-6">
            <a href="/notes" class="text-blue-500 underline">Regresar...</a>
        </p>

        <p>
            <?= htmlspecialchars($note['body']) ?>
        </p>

        <form class="mt-6" method="POST">
            <input type="hidden" name="_method" value="DELETE"> 
            <input type="hidden" name="id" value="<?= $note['id'] ?>">
            <button class="text-sm text-red-500">Eliminar</button>
        </form>

    </div>
</main>

<?php require base_path('views/partials/footer.php') ?>
```

## Código en  `Core/functions.php`
```php
<?php 

use Core\Response;

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

// Abort
function abort($code = 404){
    http_response_code($code);
    require base_path("views/{$code}.php");
    die();
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

## Compact
Podemos reducir este método:
```php
public function add($method, $uri, $controller){
	$this->routes[] =[
		'uri' => $uri,
		'controller' => $controller,
		'method' => $method
	];
}
```
A compactarlo con la función de php `compact` así:
```php
public function add($method, $uri, $controller){
	$this->routes[] = compact('method', 'uri', 'controller');
}
```

`compact(array|string $var_name, array|string|null ...$var_names)`: array
$var_name: compact() takes a variable number of parameters. Each parameter can be either a string containing the name of the variable, or an array of variable names. The array can contain other arrays of variable names inside it; compact() handles it recursively.
Create array containing variables and their values
Creates an array containing variables and their values

Quedo pendiente lo de DELETE y Crear Nota!
Listo!


