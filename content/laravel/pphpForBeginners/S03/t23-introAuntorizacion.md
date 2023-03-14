---
date: 2023-03-09T18:22:47-08:00
title: "Introducción a la autorización"
menuTitle: "23. Autorización"
draft: false
weight: 25
---

En el episodio anterior, agregamos soporte para leer todas las notas creadas por un usuario en particular. Pero, al hacerlo, introdujimos sin darnos cuenta un importante problema de seguridad. En esta lección, analizaremos la autorización, los códigos de estado y los números mágicos.

## Que aprenderemos
- Autorización
- Códigos de Status
- Numero Mágicos

En el episodio anterior, agregamos soporte para leer todas las notas creadas por un usuario en particular. Pero, al hacerlo, introdujimos sin darnos cuenta un importante problema de seguridad. En esta lección, analizaremos la autorización, los códigos de estado y los números mágicos.

Ahorita tenemos un problema con el sistema, ya que si pasamos un `id` de una `nota` que no fue escrita por usuario 1, nos la despliega también.
Ejemplo: `http://localhost:8888/note?id=3`, nos despliega la nota que fue escrita por otro usuario.

Un primer intento de solucionar esto es, en el controlador encargado de mandar la información para que la nota sea desplegada, podemos constringir el query para que `user_id` sea igual al usuario actual.
En `controllers/note.php`
```php
<?php 

$config = require('config.php');
$db = new Database($config['database']);

$heading = 'Nota';
$currentUserId = 1;

// dd($_GET['id']); // verificar que tenemos el id de la nota correcto
$note = $db->query('SELECT * FROM mis_notas.notes WHERE id = :id', [
                'id' => $_GET['id']
            ])->fetch();
// dd($note);

if (! $note){
    abort(Response::NOT_FOUND);
}

if ($note['user_id'] != $currentUserId){
    abort(Response::FORBIDDEN); //la nota fue encontrada pero no esta escrita por usuario 1
}

require "views/note.view.php";
```
Por lo pronto estamos grabándolo en código duro que el usuario es el 1.

En `views/403.php`
```php
<?php require('partials/head.php') ?>
<?php require('partials/nav.php') ?>

<main>
    <div class="mx-auto max-w-7xl py-6 sm:px-6 lg:px-8">
        <h1 class="text-2xl font-bold">No autorizado para ver esta nota!</h1>
        <p class="mt-4">
            <a href="/" class="text-blue underline">Regresar a inicio.</a>
        </p>   
    </div>
</main>

<?php require('partials/footer.php') ?>
```
En `Response.php`
```php
<?php 

class Response {
    const NOT_FOUND = 404;
    const FORBIDDEN = 403;
}
```
En `index.php`
```php
<?php 

require 'display_errors.php';

require 'functions.php';
require 'Database.php';
require 'Response.php';
require 'router.php';
```

Finalmente para poder desplegar los errores que nos manda el servidor de PHP incluir este código en `display_errors.php`
```php
<?php 

// https://stackify.com/display-php-errors/
// The quickest way to display all php errors and warnings is to add these lines to your PHP code file:
ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
error_reporting(E_ALL);
```
Listo!

