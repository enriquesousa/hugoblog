---
title: "Mostrar Posts"
menuTitle: "27. Mostrar Posts"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 15
---

### Mostrar Posts
Para mostrar el contenido de los Posts en el Frontend
Eliminar el menu de Post del menu Builder también eliminarlo de nuestro archivo de rutas, porque los vamos a mostrar directamente en el Home page de nuestro Frontend.
Vamos a crear un Controlador "PostController" para extraer los datos.
```php
lando php artisan make::controller PostController 
```
Modificar el código en:
app/Http/Controllers/PostController.php
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use PhpParser\Node\Expr\AssignOp\Pow;
use TCG\Voyager\Models\Post;

class PostController extends Controller
{
    public function index(){
        $posts = Post::paginate();
        return view('welcome', compact('posts'));
    }

    public function show(Post $post){
        return view('posts.show', compact('post'));
    }
}
```

Y en resources/views/welcome.blade.php
```php
<x-app-layout>

    <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        <h1 class="mt-4 text-4xl font-semibold text-center mb-8">{{ setting('site.title') }}</h1>

        <ul class="space-y-8">
            @foreach ($posts as $post)
                <li>
                    <article>
                        <h1 class="text-2xl font-semibold">
                            <a href="{{ route('posts.show', $post) }}">
                                {{ $post->title }}
                            </a>
                        </h1>
                        <figure>
                            <img src="{{ Voyager::image($post->image) }}" alt="" class="aspect-[3/1] object-cover">
                        </figure>
                        <p>{{ $post->excerpt }}</p>
                    </article>
                </li>
            @endforeach
        </ul>

        {{-- links de pagination --}}
        {{ $posts->links() }}

    </div>

</x-app-layout>
``` 

Y en resources/views/posts/show.blade.php
```php
<x-app-layout :title="$post->seo_title">

    @push('meta')
        <meta name="description" content="{{ $post->meta_description }}">
    @endpush

    <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        <h1 class="text-2xl font-semibold mb-4">
            {{ $post->title }}
        </h1>
        <p>{{ $post->excerpt }}</p>
        <figure class="mb-4">
            <img src="{{ Voyager::image($post->image) }}" alt="" class="aspect-[3/1] object-cover">
        </figure>
        {{-- el body tiene texto enriquecido por eso lo colocamos dentro de los signos !! --}}
        <div>
            {!! $post->body !!}
        </div>
    </div>

</x-app-layout>
```

### Meta Descripción
También podemos incluir meta descripción que esta incluido en nuestros Posts entrando en el header de resources/views/layouts/app.blade.php
```php
@stack('meta')
```
17:35
Y en resources/views/posts/show.blade.php
```php
@push('meta')
    <meta name="description" content="{{ $post->meta_description }}">
@endpush 
```

### Pasar el titulo de la pestaña
19:37
Al inicio de resources/views/layouts/app.blade.php, tambien reemplazar el title por la variable title
```php
@props(['title'] => config('app.name', 'Laravel')) 
<head>
    @stack('meta')
    <title>{{ $title }}</title>
</head>
```
Ahora para poder enviarle el nombre del title desde show post
En resources/views/posts/show.blade.php
```php
<x-app-layout title="Prueba"> 
```
Ya funciona!
Ahora para mandarle el campo de la base de datos seo_title y decirle que esto es código php se usan los dos puntos, asi:
```php
<x-app-layout :title="$post->seo_title">
```

### Definiciones
**aspect-[3/1]**
Aspecto de la imagen 3 a 1

**object-cover**
La imagen no se deforme cuando dimensionamos

**py-12**
Para que tenga una separación tanto en la parte de arriba como en la de abajo

**Símbolos {!! $post->body !!}**
El body del post tiene texto enriquecido por eso lo colocamos dentro de los signos !! para no perder la información y se puedan representar correctamente los caracteres.

