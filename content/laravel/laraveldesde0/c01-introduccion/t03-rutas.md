---
title: "Rutas en Laravel"
menuTitle: "03. Rutas"
date: 2022-10-22T18:24:33-07:00
draft: false
weight: 15
---

## Punto de entrada a nuestra aplicación.
Nuestro único punto de entrada a nuestra aplicación lo tenemos en el archivo:
**public/index.php**

## Para Administrar las rutas
Laravel nos proporciona el siguiente archivo.
**routes/web.php**

Cuando un usuario escribe una *url* laravel revisa si esta ruta esta definida en este archivo.
El código básico que ya esta por omisión es la ruta principal de la aplicación ("/")
```php
Route::get('/', function () {
    return view('welcome');
});
```
El nos regresa la pagina de Bienvenida de Laravel!
Estas paginas Laravel las maneja como vistas (views).
Ahora si quitamos este código, e intentamos ingresar a la pagina principal *http://laravel9desde0.lndo.site/*" veremos el despliegue de una vista de error diciendo, **404 | NOT FOUND** ya ue laravel encuentra que esta que esta ruta no esta definida!

Si sustituimos la vista principal welcome de Laravel y simplemente regresamos una cadena de texto asi:
```php
Route::get('/', function () {
    return "Bienvenido Pagina Principal!";
});
```
Al actualizar nuestra vista principal ya vemos como nos regresa la cadena "*Bienvenido Pagina Principal!*" que le hemos indicado.

Imaginemos que vamos a crear una plataforma de cursos, hay que implementar mas rutas, as:
```php
Route::get('/', function () {
    return "Bienvenido Pagina Principal!";
});

// nombrar una ruta
Route::get('cursos', function () {
    return "Bienvenido a la pagina de Cursos!";
});

// crear subdominio url
Route::get('cursos/create', function () {
    return "En esta pagina podrás crear un curso.";
});

// pasar mas de un parámetro por url a la ruta
// pedir que el segundo parámetro sea opcional, solo agregar el caracter ?
Route::get('cursos/{curso}/{categoria?}', function ($curso, $categoria = null) {

    // A todo este código se le conoce como lógica de la ruta, por lo cual lo tendremos que llamar después con un Controlador
    if ($categoria) {
        return "Bienvenido al curso: $curso, de la categoria $categoria.";
    } else {
        return "Bienvenido al curso: $curso";
    }

});
```

Para visitar y probar estas rutas que hemos definido en nuestro código:
- http://laravel9desde0.lndo.site/cursos
