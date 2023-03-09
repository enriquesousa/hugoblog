---
date: 2023-03-08T20:18:59-08:00
title: " Desplegar las Notas"
menuTitle: "22. Desplegar Notas"
draft: false
weight: 20
---

Ahora que tenemos la estructura inicial de la base de datos en su lugar, creemos una página para mostrar todas las notas de John Doe y luego otra página para cada nota individual.

## Ciclo de creación
Para crear una nueva entrada de menú, tenemos que pasar por un ciclo de crear una nueva entrada de menú, en este caso vamos a crear el de `notas` para poder desplegar las notas que tenemos almacenadas en la base de datos.

### Paso 1 - views/partials/nav.php
```php
<a href="/notes" class="<?= urlIs("/notes") ? 'bg-blue-900 text-white' : 'text-gray-300' ?> hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium">Notas</a>
```
### Paso 2 - router.php
Escuchar la `solicitud` del uri `/notes` y pasar el control al controlador responsable de atender esa `solicitud` (request)!
```php
$routes = [
    '/notes' => 'controllers/notes.php',
];
```
El controlador delega y prepara los datos para después pasarlos a la vista.
### Paso 3 - controllers/notes.php
```php
<?php 
$heading = 'Notas';
require "views/notes.view.php";
```
### Paso 4 - views/notes.view.php
```php
<?php require('partials/head.php') ?>
	<!-- Contenido -->
	<main>
		<div class="mx-auto max-w-7xl py-6 sm:px-6 lg:px-8">
			<p>Pagina Notas</p>
		</div>
	</main>
<?php require('partials/footer.php') ?>
```

## Desplegar unas notas 
Tenemos que preparar nuestro query dentro del controlador `controllers/notes.php`
```php
<?php 

$config = require('config.php');
$db = new Database($config['database']);

$heading = 'Notas';

$notes = $db->query('SELECT id, body, user_id FROM mis_notas.notes WHERE user_id = 1')->fetchAll();

dd($notes);

require "views/notes.view.php";
```
Nota: que por el momento movimos el cargar la base de datos aqui al inicio del controlador.

Y en `index.php`
```php
<?php 

require 'functions.php';
require 'Database.php';
require 'router.php';
```
Nota: como movimos `Database.php` antes de cargar las rutas `router.php`

Si refrescamos nuestro browser vemos como ya podemos ver las notas escritas por `user_id = 1`.

Y por ultimo para ver esas notas en nuestra vista podemos hacer un `foreach()` en la variable array `$notes`, entonces en `views/notes.view.php`:
```php
<!-- html head -->
<?php require('partials/head.php') ?>

        <!-- Navegación -->
        <?php require('partials/nav.php') ?>

        <!-- Encabezado (Banner)-->
        <?php require('partials/banner.php') ?>

        <!-- Contenido -->
        <main>
            <div class="mx-auto max-w-7xl py-6 sm:px-6 lg:px-8">

                <?php foreach ($notes as $note) : ?>

                    <li><?=$note['body'] ?></li>

                <?php endforeach; ?>

            </div>
        </main>

<?php require('partials/footer.php') ?>
```

Ahora si queremos que el contenido de las notas solo sea un extracto de la nota y nos redireccione a la nota completa.
En `views/notes.view.php`:
```php
<?php foreach ($notes as $note) : ?>
	<li>
		<a href="/note?id=<?=$note['id'] ?>" class="text-blue-500 hover:underline">
			<?=$note['body'] ?>
		</a>
	</li>
<?php endforeach; ?>
```
Pasamos por el `query string` el id de la nota, ahora creamos una nueva ruta que se llame `note` en nuestro `router.php`.
```php
$routes = [
    '/' => 'controllers/index.php',
    '/about' => 'controllers/about.php',
    '/notes' => 'controllers/notes.php',
    '/note' => 'controllers/note.php',
    '/contact' => 'controllers/contact.php'
];
```
Creamos su controlador `controllers/note.php`
```php
<?php 

$config = require('config.php');
$db = new Database($config['database']);

$heading = 'Nota';

// dd($_GET['id']); // verificar que tenemos el id de la nota correcto
$notes = $db->query('SELECT id, body, user_id FROM mis_notas.notes WHERE id = :id', ['id' => $_GET['id']])->fetchAll();

require "views/notes.view.php";
```
Nota: por ahorita usamos la misma vista `views/notes.view.php`.

Para corregir esto y solo hacer `fetch()` y no `fetchall()` modificamos la linea y le modificamos que sea otra vista, en `controllers/note.php` así:
```php
$note = $db->query('SELECT id, body, user_id FROM mis_notas.notes WHERE user_id = 1')->fetch();
require "views/note.view.php";
```
Y en `views/note.view.php`.
```php
<!-- Contenido -->
<main>
	<div class="mx-auto max-w-7xl py-6 sm:px-6 lg:px-8">
		<p><?=$note['body'] ?></p>
	</div>
</main>
```
Por ultimo, añadir un botón que nos regrese a todas las notas de este usuario, n `views/note.view.php`.
```php
<!-- Contenido -->
<main>
	<div class="mx-auto max-w-7xl py-6 sm:px-6 lg:px-8">
		<p class="mb-6">
			<a href="/notes" class="text-blue-500 underline">Regresar...</a>
		</p>
		<p><?=$note['body'] ?></p>
	</div>
</main>
```
Listo!



