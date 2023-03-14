---
date: 2023-03-14T15:00:19-07:00
title: "Espacio de nombres: ¿Qué, por qué, cómo?"
menuTitle: "31. Namespace"
draft: false
weight: 30
---

Es hora de discutir el espacio de nombres de PHP, pero no se preocupe: voy a hacer que esto sea increíblemente fácil de entender. Si puede recordar los días en que almacenaba toda su colección de música local-mente, comprenderá el espacio de nombres en segundos.

## Que aprenderemos
- PHP Namespaces
- The use Keyword

## Crear `Namespaces` Carpeta Core
Para los archivos de la carpeta `Core` (Core Files):
![Widgets](/phpforbegginers/Core-Files.png)

El `Namespace` solo declaramos para los archivos que contengan una `Clase`, en nuestro caso, todos los archivos que inician con una letra mayúscula. 

En `Core/Database.php`
```php
<?php 
namespace Core;
use PDO;
...
```
Incluimos la clase nativa de PHP con `use PDO` ya que una vez que hemos definido un `Namespace` ya todas las clases son buscadas en esa carpeta, en nuestro caso la carpeta es `Core/`. Si por alguna razón no quisieras incluir la declaración `use PDO;` puedas también mandarla llamar con la siguiente sintaxis `\PDO` así:
`$this->connection = new \PDO($dsn, $username, $password, [\PDO::ATTR_DEFAULT_FETCH_MODE => \PDO::FETCH_ASSOC]);`

En `Core/Response.php`
```php
<?php 
namespace Core;
```

En `Core/Validator.php`
```php
<?php 
namespace Core;
...
```

En `Core/functions.php` tenemos que incluir `use Core\Response` porque dentro hay una función que requiere `Response::FORBIDDEN`.
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

En `Core/display_errors.php`:
```php
<?php 

// https://stackify.com/display-php-errors/
// The quickest way to display all php errors and warnings is to add these lines to your PHP code file:
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
error_reporting(E_ALL);
```

En `Core/router.php`:
```php
<?php 

// función para route to controller
function routeToController($uri, $routes){
    if ( array_key_exists($uri, $routes) ) {
        require base_path($routes[$uri]);
    } else {
        abort();
    }
}

// función para display 404 page 
function abort($code = 404){
    http_response_code($code);
    require base_path("views/{$code}.php");
    die();
}

$routes = require base_path('routes.php');
$uri = parse_url($_SERVER['REQUEST_URI'])['path'];
routeToController($uri, $routes);
```

## En `public/index.php`
```php
<?php 
const BASE_PATH = __DIR__ . '/../';

require BASE_PATH . 'Core/functions.php';
require base_path('Core/display_errors.php');

spl_autoload_register(function ($class){
    $class = str_replace('\\', DIRECTORY_SEPARATOR, $class);
    require base_path("{$class}.php");
});

require base_path('Core/router.php');
```

## En Controladores
En `controllers/index.php`:
```php
<?php 

view("index.view.php", [
    'heading' => 'Home'
]);
```

En `controllers/contact.php`:
```php
<?php 

view("contact.view.php", [
    'heading' => 'Contact Us'
]);
```

En `controllers/about.php`:
```php
<?php 

view("about.view.php", [
    'heading' => 'About Us'
]);
```


## Controladores de notes
En `controllers/notes/index.php`:
```php
<?php 
use Core\Database;

$config = require base_path('config.php');
$db = new Database($config['database']);

$notes = $db->query('SELECT id, body, user_id FROM mis_notas.notes WHERE user_id = 1')->get();

view("notes/index.view.php", [
    'heading' => 'Mis Notas',
    'notes' => $notes,
]);
```

En `controllers/notes/show.php`:
```php
<?php 
use Core\Database;

$config = require base_path('config.php');
$db = new Database($config['database']);

$currentUserId = 1;

$note = $db->query('SELECT * FROM mis_notas.notes WHERE id = :id', [
                    'id' => $_GET['id']
                ])->findOrFail();

authorize($note['user_id'] === $currentUserId);

view("notes/show.view.php", [
    'heading' => 'Nota',
    'note' => $note,
]);
```

En `controllers/notes/create.php`:
```php
<?php 
use Core\Database;
use Core\Validator;

$config = require base_path('config.php');
$db = new Database($config['database']);

$errors = [];

if($_SERVER['REQUEST_METHOD'] === 'POST'){

    if (! Validator::string($_POST['body'], 1, 1000) ) {
        $errors['body'] = 'Se requiere contenido entre 1 y 1000 caracteres.';
    }

    if (empty($errors)) {
        
        $db->query('INSERT INTO notes (body, user_id) VALUES(:body, :user_id)', [
            'body' => $_POST['body'],
            'user_id' => 1
        ]);

    }
}

view("notes/create.view.php", [
    'heading' => 'Crear Nota',
    'errors' => $errors,
]);
```

## Vistas notes

En `views/notes/index.view.php`:
```php
<?php require base_path('views/partials/head.php') ?>
<?php require base_path('views/partials/nav.php') ?>
<?php require base_path('views/partials/banner.php') ?>
<!-- Contenido -->
<main>
	...
</main>
<?php require base_path('views/partials/footer.php') ?>
```

En `views/notes/show.view.php`:
```php
<?php require base_path('views/partials/head.php') ?>
<?php require base_path('views/partials/nav.php') ?>
<?php require base_path('views/partials/banner.php') ?>
<!-- Contenido -->
<main>
	...
</main>
<?php require base_path('views/partials/footer.php') ?>
```

En `views/notes/create.view.php`:
```php
<?php require base_path('views/partials/head.php') ?>
<?php require base_path('views/partials/nav.php') ?>
<?php require base_path('views/partials/banner.php') ?>
<!-- Contenido -->
<main>
	...
</main>
<?php require base_path('views/partials/footer.php') ?>
```

Listo!

