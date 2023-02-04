---
title: "Crear y Editar Menús"
menuTitle: "19. Crear y Editar Menús"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 5
---

### Reorganizar menus
Entrar a Tools Menu Builder, por lo pronto tenemos un solo menu admin, presionar botón verde "Constructor". Ahi podemos mover los items actuales 

Podemos re-acomodar el orden y el nombre de los menus.

Podemos mover los menus a submenus desplazando simplemente a la derecha.

### Crear nuevo menu para nuestro Front end
1. Entrar a Tools Menu Builder y dar el botón "Crear" y nombrarlo "main".
2. Entrar al botón "Constructor" y empezar a crear los enlaces que va a tener nuestro menu.
3. Para los enlaces del frontend primero tenemos que definir nuestras rutas en web.php
```php
 Route::get('/', function () {
    return view('welcome');
})->name('welcome');

Route::get('/posts', function () {
    // vista
})->name('posts.index');

Route::get('/posts/{post}', function () {
    // vista
})->name('posts.show');

Route::get('/about', function () {
    // vista
})->name('about');

Route::get('/policy', function () {
    // vista
})->name('policy');
```

Por lo pronto ya tenemos algunas rutas de prueba para nuestro frontend, ahora como podemos utilizarlas desde Voyager menu builder.
Ya podemos "Nueva opción de menu" para nuestro menu.
1. Si utilizamos ruta dinámica, podemos utilizar el nombre de la ruta.
2. Para las rutas que necesitan que le pasemos un parámetro.
![Widgets](/Voyager/crearmenu-welcome.png)

Asi agregar los demás menus.

### Como utilizar este menu en el frontend
Eliminar el código que viene por omisión en resources/views/welcome.blade.php e utilizar la plantilla de menu que viene con jetstream, es la plantilla que se muestra cuando entramos al dashboard.

Comentar el menu de jetstream en resources/views/layouts/app.blade.php y re-emplazarlo con el nuevo menu de Voyager asi:
```php
<body class="font-sans antialiased">
    <x-jet-banner />

    <div class="min-h-screen bg-gray-100">
        {{-- @livewire('navigation-menu') --}}
        {{ menu('main') }}

        <!-- Page Heading -->
        @if (isset($header))
            <header class="bg-white shadow">
                <div class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8">
                    {{ $header }}
                </div>
            </header>
        @endif

        <!-- Page Content -->
        <main>
            {{ $slot }}
        </main>
    </div>

    @stack('modals')

    @livewireScripts
</body> 
```
Nota: Re-emplazar el menu con el de Voyager agregando solo.
```php
{{ menu('main') }}
```
Listo!
Simplemente nos da una lista con cada uno de nuestros enlaces, todavía no aplica estilo.
Los estilos los vemos en el proximo tema!








