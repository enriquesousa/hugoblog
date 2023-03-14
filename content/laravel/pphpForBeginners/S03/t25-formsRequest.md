---
date: 2023-03-11T19:25:33-08:00
title: "Intro Forms and Request"
menuTitle: "25. Forms and Request"
draft: false
weight: 35
---

Estamos atrasados, pero finalmente es hora de profundizar en los formularios. En esta lección, aprenderemos cómo enviar un formulario utilizando dos métodos de solicitud diferentes. A continuación, analizaremos cómo nuestro controlador podría detectar si se ha producido un envío POST.

## Que aprenderemos
- Forms
- GET Requests
- POST Requests

Refactorizar el código de `router.php` para que quede mas limpio y poder tenet en su propio archivo a solo las rutas en un archivo que llamaremos `routes.php`. 
En `router.php`
```php
<?php 

$routes = require('routes.php');

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

$uri = parse_url($_SERVER['REQUEST_URI'])['path'];
routeToController($uri, $routes);
```
En `routes.php`
```php
<?php 

return [
    '/' => 'controllers/index.php',
    '/about' => 'controllers/about.php',
    '/notes' => 'controllers/notes.php',
    '/note' => 'controllers/note.php',
    '/contact' => 'controllers/contact.php'
];
```

Colocar un botón para agregar notas en la vista `views/notes.view.php`
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

                <form method="POST">

                    <label for="body">Descripción</label>
                    <div>
                        <textarea name="body" id="body" cols="30" rows="10"></textarea>
                    </div>

                    <p>
                        <button type="submit">Crear</button>
                    </p>

                </form>

            </div>
        </main>

<?php require('partials/footer.php') ?>
```
En `controllers/note-create.php`
```php
<?php 

$heading = 'Crear Nota';

if($_SERVER['REQUEST_METHOD'] === 'POST'){
    dd($_POST);
}

require 'views/note-create.view.php';
```
Por ahorita vemos como podemos mandar la información de `body` con el método `POST`.

Salida de `dd($_POST)`:
```php
array(1) {
  ["body"]=>
  string(6) "prueba"
}
```

Ahora vamos a cambiar nuestra forma NO estilizada por una que nos ofrezca [Tailwind CSS](https://tailwindui.com), hacer una búsqueda de `forms` [link](https://tailwindui.com/components/application-ui/forms/form-layouts).
Sustituirlo y limpiarlo y quedarnos solo con lo que nos interesa.

Al ver la información al inicio de la forma nos dice que requiere un plugin de `require('@tailwindcss/forms')`:
```php
<!--
This example requires some changes to your config:

// tailwind.config.js
module.exports = {
	// ...
	plugins: [
	// ...
	require('@tailwindcss/forms'),
	],
}
-->
```
Como nosotros estamos usan el `CDN` de Tailwind en `views/partials/head.php` podemos incluir el plug in de la siguiente manera:
`<script src="https://cdn.tailwindcss.com?plugins=forms"></script>`
Con esto ya podemos visualizar correctamente la forma de tailwind.

Entonces nos queda en `views/note-create.view.php`
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

        <!-- Sección 1 - Profile -->
        <div>
            <div class="md:grid md:grid-cols-3 md:gap-6">

                <div class="mt-5 md:col-span-2 md:mt-0">
                    <form method="POST">

                        <div class="shadow sm:overflow-hidden sm:rounded-md">
                            <div class="space-y-6 bg-white px-4 py-5 sm:p-6">

                                <!-- Body -->
                                <div>
                                    <label for="body" class="block text-sm font-medium leading-6 text-gray-900">Body</label>
                                    <div class="mt-2">
                                        <textarea id="body" name="body" rows="3" class="mt-1 block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:py-1.5 sm:text-sm sm:leading-6" placeholder="Aquí una idea para tu nota! ..."></textarea>
                                    </div>
                                </div>

                            </div>
                            <div class="bg-gray-50 px-4 py-3 text-right sm:px-6">
                                <button type="submit" class="inline-flex justify-center rounded-md bg-indigo-600 py-2 px-3 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-500">Salvar Nota</button>
                            </div>
                        </div>

                    </form>
                </div>

            </div>
        </div>


    </div>
</main>

<?php require('partials/footer.php') ?>
```
Listo!
Si llenamos con datos el text area y damos click al botón de Salvar Nota (submit), la información viaja hacia el controlador y nos despliega la información enviada con `dd($_POST);`.

Nota: Como no hemos todavía incluido una re-dirección (action='') a otra pagina en:
```php
<form action="" method="POST"></form>
```
Cuando no incluimos `action` en la forma, significa que vamos a re-dirigir el submit a la misma pagina en donde estamos, por eso es que lo podemos recibir en nuestro controlador `controllers/note-create.php`.


