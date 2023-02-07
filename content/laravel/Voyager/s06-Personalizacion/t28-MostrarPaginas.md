---
title: "Mostrar Páginas"
menuTitle: "28. Mostrar Páginas"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 20
---

### Primero Crear las paginas
Dentro de Voyager en el Side Bar ir a "Pages", modificar la pagina para que se llame "Nosotros" también cambiar el slug a "about", y crear una pagina adicional con el botón "Crear":
Name: "Políticas y Privacidad"
Slug: "policy"
Meta Description: "Mauris id nulla a tortor viverra dictum eu vitae urna."
Meta Keywords: "prueba1"
Status: "Active"
No image

### Desplegar las paginas en nuestro frontend
Crear un controlador:
```php
lando php artisan make::controller PageController
```
Implementar el siguiente código.
routes/web.php
```php
use App\Http\Controllers\PageController;

Route::get('/about', [PageController::class, 'about'])->name('about');
Route::get('/policy', [PageController::class, 'policy'])->name('policy'); 
```
En app/Http/Controllers/PageController.php
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use TCG\Voyager\Models\Page;

class PageController extends Controller
{
    public function about(){
        $page = Page::where('slug', 'about')->firstOrFail();
        return view('pages.about', compact('page'));
    }

    public function policy(){
        $page = Page::where('slug', 'policy')->firstOrFail();
        return view('pages.policy', compact('page'));
    }
}
```
En resources/views/pages/about.blade.php
```php
<x-app-layout>

    <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-12">

        {{-- Titulo de la pagina --}}
        <h1 class="text-4xl font-semibold text-center mb-4">
            {{ $page->title }}
        </h1>

        {{-- Body --}}
        <div class="mb-4">
            {!! $page->body !!}
        </div>

        {{-- Imagen --}}
        <figure>
            <img src="{{ Voyager::image($page->image) }}" alt="" class="aspect-[4/3] object-cover">
        </figure>

    </div>

</x-app-layout>
```
En resources/views/pages/policy.blade.php
```php
<x-app-layout>

    <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-12">

        {{-- Titulo de la pagina --}}
        <h1 class="text-4xl font-semibold text-center mb-4">
            {{ $page->title }}
        </h1>

        {{-- Body --}}
        <div class="mb-4">
            {!! $page->body !!}
        </div>

        {{-- Si tenemos imagen mostramos la imagen --}}
        @if ($page->image)
            {{-- Imagen --}}
            <figure>
                <img src="{{ Voyager::image($page->image) }}" alt="" class="aspect-[4/3] object-cover">
            </figure>
        @endif

    </div>

</x-app-layout>
```

### Notas Finales
Lo interesante de todo esto es, que con Voyager podemos entrar y modificar el contenido de nuestras paginas, y lo mas interesante, que podemos darle acceso a un usuario con restricción de roles y permisos para que el cliente mismo pueda modificar sus contenidos!
