---
date: 2023-03-16T22:50:25-07:00
title: "Un Request un Controlador"
menuTitle: "34. Request Controller"
draft: false
weight: 45
---

Con un enrutador mejorado que puede responder a múltiples tipos de solicitudes de formularios, ahora podemos refactorizar las acciones de nuestro controlador para que estén más enfocados. También discutiremos la importancia de mantener las acciones de su controlador lo más simples posible.

## Que aprenderemos
- Controller Actions
- Request Methods

## Implementar código
Vamos a implementar el código en los siguientes controles.
```php
$router->delete('/note', 'controllers/notes/destroy.php');
$router->get('/notes/create', 'controllers/notes/create.php');
$router->post('/notes', 'controllers/notes/store.php');
```
En `controllers/notes/create.php` ya solo desplegaremos la forma:
```php
<?php 

// Solo desplegar la forma para crear una nota
view("notes/create.view.php", [
    'heading' => 'Crear Nota',
    'errors' => [],
]);
```

En `views/notes/create.view.php`:
Redirecionamos ahora con `action="/notes"` a `$router->post('/notes', 'controllers/notes/store.php');` para que nos lleve al código de `Salvar Nota`:
```php
..
<form method="POST" action="/notes">
	..
</form>
..
```

En `routes.php`
```php
<?php 

$router->get('/', 'controllers/index.php');
$router->get('/about', 'controllers/about.php');
$router->get('/contact', 'controllers/contact.php');

$router->get('/notes', 'controllers/notes/index.php');
$router->get('/note', 'controllers/notes/show.php');
$router->delete('/note', 'controllers/notes/destroy.php');

$router->get('/notes/create', 'controllers/notes/create.php');
$router->post('/notes', 'controllers/notes/store.php');
```

En `controllers/notes/store.php`
```php
<?php

use Core\Database;
use Core\Validator;

$config = require base_path('config.php');
$db = new Database($config['database']);

$errors = [];

// Validar user input
if (! Validator::string($_POST['body'], 1, 1000) ) {
    $errors['body'] = 'Se requiere contenido entre 1 y 1000 caracteres.';
}

// si hay problema con la validación
if (! empty($errors)) {
    // Regresar la vista
    return view("notes/create.view.php", [
        'heading' => 'Crear Nota',
        'errors' => $errors,
    ]);
}

// Si no hay errores podemos insertar a db
if (empty($errors)) {
    
    $db->query('INSERT INTO notes (body, user_id) VALUES(:body, :user_id)', [
        'body' => $_POST['body'],
        'user_id' => 1
    ]);

    // Redirect a la pagina de show notes
    header('location: /notes');
    die();
}
```

Y para DELETE una Nota.
Primero mostramos la nota con `controllers/notes/show.php`:
```php
<?php 

use Core\Database;

$config = require base_path('config.php');
$db = new Database($config['database']);

// current user id (hard code)
$currentUserId = 1; 

// dd($_GET['id']); // verificar que tenemos el id de la nota correcto
$note = $db->query('SELECT * FROM mis_notas.notes WHERE id = :id', [
                    'id' => $_GET['id']
                ])->findOrFail();
authorize($note['user_id'] === $currentUserId);

view("notes/show.view.php", [
    'heading' => 'Nota',
    'note' => $note,
]);
```
La Nota se muestra con la vista `views/notes/show.view.php`:
Aquí con el botón `Eliminar` dentro de la forma que es `POST` porque las formas solo pueden enviar POST o GET, pero por medio de truco de enviar datos hidden podemos redirigir trafico con nuestro router 
```php
...
<form class="mt-6" method="POST">
	<input type="hidden" name="_method" value="DELETE"> 
	<input type="hidden" name="id" value="<?= $note['id'] ?>">
	<button class="text-sm text-red-500">Eliminar</button>
</form>
...
```


# Recorrido `DELETE` de una Nota

1. Todo inicia en `public/index.php`
1a. Cargamos las funciones `Core/functions.php`
1b. Inicial-izamos la clase `$router = new \Core\Router()`
1c. Cargamos un array asociativa bi-dimensional (global variable) $routes que va a contener todos los routes definidos en `routes.php`
 
	```php
	object(Core\Router)[3]
	  protected 'routes' => 
		array (size=8)
		  0 => 
			array (size=3)
			  'method' => string 'GET' (length=3)
			  'uri' => string '/' (length=1)
			  'controller' => string 'controllers/index.php' (length=21)
		  1 => 
			array (size=3)
			  'method' => string 'GET' (length=3)
			  'uri' => string '/about' (length=6)
			  'controller' => string 'controllers/about.php' (length=21)
		  2 => 
			array (size=3)
			  'method' => string 'GET' (length=3)
			  'uri' => string '/contact' (length=8)
			  'controller' => string 'controllers/contact.php' (length=23)
		  3 => 
			array (size=3)
			  'method' => string 'GET' (length=3)
			  'uri' => string '/notes' (length=6)
			  'controller' => string 'controllers/notes/index.php' (length=27)
		  4 => 
			array (size=3)
			  'method' => string 'GET' (length=3)
			  'uri' => string '/note' (length=5)
			  'controller' => string 'controllers/notes/show.php' (length=26)
		  5 => 
			array (size=3)
			  'method' => string 'DELETE' (length=6)
			  'uri' => string '/note' (length=5)
			  'controller' => string 'controllers/notes/destroy.php' (length=29)
		  6 => 
			array (size=3)
			  'method' => string 'GET' (length=3)
			  'uri' => string '/notes/create' (length=13)
			  'controller' => string 'controllers/notes/create.php' (length=28)
		  7 => 
			array (size=3)
			  'method' => string 'POST' (length=4)
			  'uri' => string '/notes' (length=6)
			  'controller' => string 'controllers/notes/store.php' (length=27)

	```
	
2. En este mismo archivo de `public/index.php` cargamos las variables `$uri y $method` utilizando las variables super globales `$_SERVER['REQUEST_URI']` y `$_SERVER['REQUEST_METHOD']`
3. Y mandamos llamar al método `$router->route($uri, $method)` de la clase `Route` para que nos redirija al controlador deseado dentro de nuestra estructura de código, efectivamente requiriendo ese controlador con el método de la clase y la función: 
	 ```php
	public function route($uri, $method){
			foreach ($this->routes as $route) {
				if ($route['uri'] === $uri && $route['method'] === strtoupper($method) ) {
					return require base_path($route['controller']);
				}
			}
			$this->abort();
		}
	```
	 ```php
	function base_path($path){
		return BASE_PATH . $path;
	}
	```
4. En caso de haber escogido la opción del menu de navegación de ver las Notas, entonces somos redirigidos al controlador `controllers/notes/index.php
5. El cual se encarga de desplegar todas las notas del usuario autorizado , por el momento hard code user_id = 1 con el query `$notes = $db->query('SELECT id, body, user_id FROM mis_notas.notes WHERE user_id = 1')->get();`
6. De ahi desplegamos sus Notas con la vista `views/notes/index.view.php`
7. Aquí en la vista tenemos la opción de Crear una Nota nueva y/o ver una de las notas ya registradas en la base de datos. 
8. Vamos a entrar a ver una de las notas para que tengamos la oportunidad de `Eliminar` la nota.
9. Al dar click a una de las notas (`<a href="/note?id=<?=$note['id'] ?>" class="text-blue-500 hover:underline">`) nos redirige a través de nuestro `Router` a la ruta 	`'/note', 'controllers/notes/show.php'` el cual se encarga de desplegar la nota.
10. El cual se encarga de preparar la nota con el id enviado.
	```php
	$note = $db->query('SELECT * FROM mis_notas.notes WHERE id = :id', [
						'id' => $_GET['id']
					])->findOrFail();
	```
11. Una vez cargadas la nota y haber verificado que sea el usuario autorizado, podemos proseguir a mostrar la nota en la vista `views/notes/show.view.php` 
12. En esta vista tenemos un botón para poder `Eliminar` la nota.
13. El código para el botón Eliminar lo tenemos dentro de una forma en donde podemos enviar por un campo escondido el método `DELETE`:
	```php
	<form class="mt-6" method="POST">
		<input type="hidden" name="_method" value="DELETE"> 
		<input type="hidden" name="id" value="<?= $note['id'] ?>">
		<button class="text-sm text-red-500">Eliminar</button>
	</form>
	```
14. El cual que podemos escuchar con nuestro `Router`.
15. Con el hidden method **DELETE** nuestro Router nos redirecciona a:
16. `$router->delete('/note', 'controllers/notes/destroy.php');`
17. En el controlador `controllers/notes/destroy.php` 
18. Probamos primero que la nota sea del usuario autenticado
19. Después eliminamos la nota con query:
	```php
	// Delete the current note
	$db->query('DELETE FROM notes WHERE id = :id', [
		'id' => $_GET['id'] 
	]);
	``` 
20. Redirigimos al usuario a la pagina de notas
Listo!



