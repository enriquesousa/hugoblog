---
title: "Cómo se renderizan los componentes de Livewire"
date: 2023-02-14T12:08:27-08:00
draft: false
weight: 10
menuTitle: "02. Cómo se renderizan los componentes de Livewire"
---

Crear nuestro primer componente de livewire:
- lando php artisan make:livewire ShowPosts

Se crea app/Http/Livewire/ShowPosts.php donde el método mas importante es render():
```php
public function render()
{
    return view('livewire.show-posts');
}
```
Se encarga de renderizar la vista que se encuentra en 'livewire.show-posts', con esto, aseguramos la reactividad de nuestro proyecto, porque cada vez que se haga una petición al servidor se va a volver a renderizar el componente de livewire.

resources/views/livewire/show-posts.blade.php:
```php
<div>
  <h1>Hola mundo!</h1>
</div>
```
Todo tiene que quedar dentro de un <div> padre.

Para mandar llamar un componente de livewire ir a:
resources/views/dashboard.blade.php:
```php
<div class="py-12">
    <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
        
        @livewire('show-posts')

    </div>
</div>
```

Para organizar los componentes dentro de carpetas, creamos el componente asi:
- lando php artisan make:livewire Nav/ShowPosts

Ya me crea el componente app/Http/Livewire/Nav/ShowPosts.php y la vista resources/views/livewire/nav/show-posts.blade.php.
Y para mandarla llamar usamos @livewire('nav.show-posts') en resources/views/dashboard.blade.php.

Para pasar parámetros al componente, lo hacemos atrevés de un array [atributo => valor]:
resources/views/dashboard.blade.php:
```php
@livewire('show-posts', ['title' => 'Este es un titulo de prueba'])
```
Para recibir la información, agregar una propiedad en app/Http/Livewire/ShowPosts.php:
```php
public $title;
public function render()
{
    return view('livewire.show-posts');
}
```
De esta manera cualquier propiedad que asignemos aquí, podrá ser accedida por resources/views/livewire/show-posts.blade.php:
```php
<div>
  <h1>Hola mundo!</h1>
  {{ $title }}
</div>
```

Ahora, para poder recibir las propiedades en app/Http/Livewire/ShowPosts.php con otro nombre, por ejemplo $titulo en vez de $title, entonces tenemos que crear un nuevo método llamado mount():
```php
public $titulo;

public function mount($title){
    $this->titulo = $title;
}

public function render()
{
    return view('livewire.show-posts');
}
```
Y para desplegar la propiedad en la vista:
```php
<div>
  <h1>Hola mundo!</h1>
  {{ $titulo }}
</div>
```

Ahora queremos usar a los componentes como si fueran controladores, para asi poder incluirlos en nuestras vistas y tener su reactividad en todas nuestras paginas. Esto se hace asi, en web.php importamos a:
web.php:
```php
<?php
use Illuminate\Support\Facades\Route;
use App\Http\Livewire\ShowPosts;
Route::get('/', function () {
    return view('welcome');
});
Route::middleware(['auth:sanctum', 'verified'])->get('/dashboard', ShowPosts::class)->name('dashboard');
```
15:00
Al utilizar livewire como un controlador ...
Podemos utilizar {{ $slot }} con nombres.
Creamos nuestra propia plantilla base.blade.php basada en app.blade.php para poder utilizarla y la modificamos.
resources/views/layouts/base.blade.php:
```php
<!DOCTYPE html>
<html lang="{{ str_replace('_', '-', app()->getLocale()) }}">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <meta name="csrf-token" content="{{ csrf_token() }}">

        <title>{{ config('app.name', 'Laravel') }}</title>

        <!-- Fonts -->
        <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Nunito:wght@400;600;700&display=swap">

        <!-- Styles -->
        <link rel="stylesheet" href="{{ mix('css/app.css') }}">

        @livewireStyles

        <!-- Scripts -->
        <script src="{{ mix('js/app.js') }}" defer></script>
    </head>
    <body class="font-sans antialiased">
        <x-jet-banner />

        <div class="min-h-screen bg-gray-100">
            @livewire('navigation-menu')

            <!-- Page Heading -->
            @if (isset($header))
                <header class="bg-white shadow">
                    <div class="max-w-7xl mx-auto py-6 px-4 sm:px-6 lg:px-8">
                        {{ $header }}
                    </div>
                </header>
            @endif
            
            <h1>Plantilla Base</h1>
            <!-- Page Content -->
            <main>
                {{ $slot }}
            </main>
        </div>

        @stack('modals')

        @livewireScripts
    </body>
</html>
```
Para redirigir el componente a nuestra nueva plantilla en app/Http/Livewire/ShowPosts.php:
```php
public function render()
{
    return view('livewire.show-posts')
            ->layout('layouts.base');
}
```
Ahora si estamos extendiendo desde nuestra plantilla base.
Esto solo fue de prueba, eliminar la plantilla base!

Como podemos pasar parámetros por la url.
agregar ruta en, web.php:
```php
Route::get('prueba/{name}', ShowPosts::class);
```
En app/Http/Livewire/ShowPosts.php:
```php
<?php
namespace App\Http\Livewire;
use Livewire\Component;
class ShowPosts extends Component
{
    public $name;
    public function mount($name){
        $this->name = $name;
    }
    public function render()
    {
        return view('livewire.show-posts');
    }
}
```
en resources/views/livewire/show-posts.blade.php:
```php
<div>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Dashboard') }}
        </h2>
    </x-slot>
    <h1>Hola mundo!</h1>
    {{ $name }}
</div>
```
Al visitar:
- http://livewire.lndo.site/prueba/enrique
Ya tenemos empresa la variable nombre

Llenar de datos de prueba nuestra bas de datos.
```bash
lando php artisan make:model Post -mf 
```
En database/migrations/2023_02_13_062424_create_posts_table.php
```php
public function up()
{
    Schema::create('posts', function (Blueprint $table) {
        $table->id();

        $table->string('title');
        $table->string('content');

        $table->timestamps();
    });
} 
```
En database/factories/PostFactory.php
```php
public function definition()
{
    return [
        'title' => $this->faker->sentence(),
        'content' => $this->faker->text()
    ];
} 
```
Ahora llena con 100 registros falsos
En database/seeders/DatabaseSeeder.php
```php
public function run()
{
    \App\Models\Post::factory(100)->create();
} 
```
En app/Models/Post.php
```php
class Post extends Model
{
    use HasFactory;
    protected $fillable = ['title', 'content'];
} 
```
Por ultimo volver a generar las tablas con ejecución de seeders para que llene los campos que acabamos de definir.
```bash
lando php artisan migrate:fresh --seed 
```

Para mostrar los 100 registros en app/Http/Livewire/ShowPosts.php
Importar el modelo de Post y en una variable llamada $posts rescatar todos los registros.
```php
<?php
namespace App\Http\Livewire;

use Livewire\Component;
use App\Models\Post;
class ShowPosts extends Component
{
    public function render()
    {
        $posts = Post::all();
        return view('livewire.show-posts', compact('posts'));
    }
}
```
Y por ultimo en la vista mostrar esa información.
En resources/views/livewire/show-posts.blade.php
```php
<div>
    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Dashboard') }}
        </h2>
    </x-slot>
    {{-- <h1>Hola mundo!</h1> --}}

    {{ $posts }}

</div>
```
Listo!

Hay que volver a registrar el user porque perdimos los datos al hacer el migrate:fresh
Listo!