---
title: "Vinculación de datos, Factories y Seeders"
date: 2023-02-14T12:08:50-08:00
draft: false
weight: 15
menuTitle: "03. Vinculación de datos, Factories y Seeders"
---

### Crear el seeder para user
- lando php artisan make:seeder UserSeeder

En database/seeders/UserSeeder.php:
```php
<?php
namespace Database\Seeders;

use App\Models\User;
use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;

class UserSeeder extends Seeder
{
    public function run()
    {
        // Crear un usuario de prueba con todo y su password
        User::create([
            'name' => 'Enrique Sousa',
            'email' => 'enrique.sousa@gmail.com',
            'password' => bcrypt('sousa1234'),
        ]);
    }
}
```
En database/seeders/DatabaseSeeder.php:
```php
public function run()
{
    // \App\Models\User::factory(10)->create();
    $this->call(UserSeeder::class);
}
```
Como recuperar información de la base de datos y pasarlo a la vista:
para esto primero hay que llenar de algunos datos la base de datos.
Crear el modelo Post con migraciones y factories.
- lando php artisan make:model Post -mf
En database/migrations/2022_02_13_172445_create_posts_table.php:
```php
Schema::create('posts', function (Blueprint $table) {
  $table->id();

  $table->string('title');
  $table->string('content');

  $table->timestamps();
});
```
En database/factories/PostFactory.php:
```php
public function definition()
{
  return [
      'title' => $this->faker->sentence(),
      'content' => $this->faker->text(),
  ];
}
```
En database/seeders/DatabaseSeeder.php:
```php
public function run()
{
  \App\Models\Post::factory(100)->create();
}
```
En app/Models/Post.php:
```php
protected $fillable = [
  'title',
  'content',
];
```
Por ultimo migrar fresh con seeders!
- lando php artisan migrate:fresh --seed
Listo!
Ahora para pasar los datos a la vista del componente.
En app/Http/Livewire/ShowPosts.php:
```php
<?php
namespace App\Http\Livewire;
use App\Models\Post;
use Livewire\Component;
class ShowPosts extends Component
{
  public function render()
  {
      $posts = Post::all();
      return view('livewire.show-posts', compact('posts'));
  }
}
```
Y en la vista resources/views/livewire/show-posts.blade.php:
```php
<div>
  <x-slot name="header">
      <h2 class="font-semibold text-xl text-gray-800 leading-tight">
          {{ __('Dashboard') }}
      </h2>
  </x-slot>

  <h1>Hola mundo!</h1>
  {{ $posts }}
</div>
```
Listo!
Volver a registrarnos en nuestro proyecto y Listo!
Ya vemos en la vista todos los 100 registros raw!

Centrar el contenido de los datos recuperados de la base de datos.
usando las clases de Tailwind tal como las usan en la vista navigation-menu.blade.php, entonces:
en resources/views/livewire/show-posts.blade.php:
```php
<div>
  <x-slot name="header">
    <h2 class="font-semibold text-xl text-gray-800 leading-tight">
      {{ __('Dashboard') }}
    </h2>
  </x-slot>

  <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
    {{ $posts }}
  </div>
</div>
```
Listo!

Para bajar una tabla de tailwind, podemos buscar en google como tailwind ui components table y nos va a salir varias paginas, yo escogí esta tabla: 
https://tailwindcomponents.com/component/customers-table

All Tailwind CSS Components
Explore our whole collection of over 600+ free UI components
https://tailwindcomponents.com/components
https://tailwindcomponents.com/awesome
https://tailwindcomponents.com/component/list-order-product


### Table component tailwind Original Copy 
{{< details "Código" >}}
    
<!-- table component tailwind Original Copy from https://tailwindcomponents.com/component/list-order-product -->
<div class="bg-white p-8 rounded-md w-full">

    {{-- Header General - Titulo, Búsqueda y Botones --}}
    <div class=" flex items-center justify-between pb-6">

        {{-- Titulo de la tabla Justificada a la Izquierda --}}
        <div>
            <h2 class="text-gray-600 font-semibold">Productos</h2>
            <span class="text-xs">All products item</span>
        </div>

        <div class="flex items-center justify-between">
            {{-- Rectángulo de búsqueda (search) --}}
            <div class="flex bg-gray-50 items-center p-2 rounded-md">
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" viewBox="0 0 20 20"
                    fill="currentColor">
                    <path fill-rule="evenodd"
                        d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z"
                        clip-rule="evenodd" />
                </svg>
                <input class="bg-gray-50 outline-none ml-1 block " type="text" name="" id=""
                    placeholder="search...">
            </div>
            {{-- Botones a la Derecha New y Create --}}
            <div class="lg:ml-40 ml-10 space-x-8">
                <button
                    class="bg-indigo-600 px-4 py-2 rounded-md text-white font-semibold tracking-wide cursor-pointer">New
                    Report</button>
                <button
                    class="bg-indigo-600 px-4 py-2 rounded-md text-white font-semibold tracking-wide cursor-pointer">Create</button>
            </div>
        </div>

    </div>

    {{-- Contenido de la Tabla --}}
    <div>
        <div class="-mx-4 sm:-mx-8 px-4 sm:px-8 py-4 overflow-x-auto">
            <div class="inline-block min-w-full shadow rounded-lg overflow-hidden">

                <table class="min-w-full leading-normal">
                    {{-- Header de la Tabla --}}
                    <thead>
                        <tr>
                            <th
                                class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
                                Name
                            </th>
                            <th
                                class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
                                products
                            </th>
                            <th
                                class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
                                Created at
                            </th>
                            <th
                                class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
                                QRT
                            </th>
                            <th
                                class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
                                Status
                            </th>
                        </tr>
                    </thead>

                    {{-- Cuerpo de la Tabla --}}
                    <tbody>

                        {{-- Renglón 1 --}}
                        <tr>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <div class="flex items-center">
                                    <div class="flex-shrink-0 w-10 h-10">
                                        <img class="w-full h-full rounded-full"
                                            src="https://images.unsplash.com/photo-1494790108377-be9c29b29330?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2.2&w=160&h=160&q=80"
                                            alt="" />
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-gray-900 whitespace-no-wrap">
                                            Vera Carpenter
                                        </p>
                                    </div>
                                </div>
                            </td>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">Admin</p>
                            </td>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">
                                    Jan 21, 2020
                                </p>
                            </td>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">
                                    43
                                </p>
                            </td>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <span
                                    class="relative inline-block px-3 py-1 font-semibold text-green-900 leading-tight">
                                    <span aria-hidden
                                        class="absolute inset-0 bg-green-200 opacity-50 rounded-full"></span>
                                    <span class="relative">Activo</span>
                                </span>
                            </td>
                        </tr>

                        {{-- Renglón 2 --}}
                        <tr>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <div class="flex items-center">
                                    <div class="flex-shrink-0 w-10 h-10">
                                        <img class="w-full h-full rounded-full"
                                            src="https://images.unsplash.com/photo-1500648767791-00dcc994a43e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2.2&w=160&h=160&q=80"
                                            alt="" />
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-gray-900 whitespace-no-wrap">
                                            Blake Bowman
                                        </p>
                                    </div>
                                </div>
                            </td>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">Editor</p>
                            </td>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">
                                    Jan 01, 2020
                                </p>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">
                                    77
                                </p>
                            </td>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <span
                                    class="relative inline-block px-3 py-1 font-semibold text-green-900 leading-tight">
                                    <span aria-hidden
                                        class="absolute inset-0 bg-green-200 opacity-50 rounded-full"></span>
                                    <span class="relative">Activo</span>
                                </span>
                            </td>
                        </tr>

                        {{-- Renglón 3 --}}
                        <tr>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <div class="flex items-center">
                                    <div class="flex-shrink-0 w-10 h-10">
                                        <img class="w-full h-full rounded-full"
                                            src="https://images.unsplash.com/photo-1540845511934-7721dd7adec3?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2.2&w=160&h=160&q=80"
                                            alt="" />
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-gray-900 whitespace-no-wrap">
                                            Dana Moore
                                        </p>
                                    </div>
                                </div>
                            </td>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">Editor</p>
                            </td>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">
                                    Jan 10, 2020
                                </p>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">
                                    64
                                </p>
                            </td>
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <span
                                    class="relative inline-block px-3 py-1 font-semibold text-orange-900 leading-tight">
                                    <span aria-hidden
                                        class="absolute inset-0 bg-orange-200 opacity-50 rounded-full"></span>
                                    <span class="relative">Suspended</span>
                                </span>
                            </td>
                        </tr>

                        {{-- Renglón 4 --}}
                        <tr>
                            <td class="px-5 py-5 bg-white text-sm">
                                <div class="flex items-center">
                                    <div class="flex-shrink-0 w-10 h-10">
                                        <img class="w-full h-full rounded-full"
                                            src="https://images.unsplash.com/photo-1522609925277-66fea332c575?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2.2&h=160&w=160&q=80"
                                            alt="" />
                                    </div>
                                    <div class="ml-3">
                                        <p class="text-gray-900 whitespace-no-wrap">
                                            Alonzo Cox
                                        </p>
                                    </div>
                                </div>
                            </td>
                            <td class="px-5 py-5 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">Admin</p>
                            </td>
                            <td class="px-5 py-5 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">Jan 18, 2020</p>
                            </td>
                            <td class="px-5 py-5 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">70</p>
                            </td>
                            <td class="px-5 py-5 bg-white text-sm">
                                <span
                                    class="relative inline-block px-3 py-1 font-semibold text-red-900 leading-tight">
                                    <span aria-hidden
                                        class="absolute inset-0 bg-red-200 opacity-50 rounded-full"></span>
                                    <span class="relative">Inactive</span>
                                </span>
                            </td>
                        </tr>
                       
                    </tbody>
                </table>

                {{-- Footer de la Tabla --}}
                <div class="px-5 py-5 bg-white border-t flex flex-col xs:flex-row items-center xs:justify-between">
                    <span class="text-xs xs:text-sm text-gray-900">
                        Showing 1 to 4 of 50 Entries
                    </span>
                    <div class="inline-flex mt-2 xs:mt-0">
                        <button
                            class="text-sm text-indigo-50 transition duration-150 hover:bg-indigo-500 bg-indigo-600 font-semibold py-2 px-4 rounded-l">
                            Prev
                        </button>
                        &nbsp; &nbsp;
                        <button
                            class="text-sm text-indigo-50 transition duration-150 hover:bg-indigo-500 bg-indigo-600 font-semibold py-2 px-4 rounded-r">
                            Next
                        </button>
                    </div>
                </div>

            </div>
        </div>
    </div>

</div>  

{{< /details >}}

### Para formatear el código

Para formatear el código instalar la extension de vscode 'laravel blade formatter' y para usarla hacer click derecho en el documento que queremos format y escoger 'Format Document With ...' y escogemos Laravel Blade Formatter, también podemos acceder a esta opción con ctrl+shift+p y buscar Format Document ...
2:48
pasar los tres primeros div's antes de la tabla a un componente blade para que no nos haga ruido visual.

Todos los div's con sus clases los vamos a pasar a un componente de blade para separarlos y quedarnos con el menor ruido visual posible en nuestro código en resources/views/livewire/show-posts.blade.php.

Para mover creamos:
resources/views/components/table.blade.php:
```php
<section class="container mx-auto p-6 font-mono">
    <div class="w-full mb-8 overflow-hidden rounded-lg shadow-lg">
        <div class="w-full overflow-x-auto">
            
            {{ $slot }}

        </div>
    </div>
</section>
```
Y en resources/views/livewire/show-posts.blade.php:
```php
...
<!-- component -->
<x-table>
    <table class="w-full">
    ...
    </table>
</x-table>
```
Se manda llamar el $slot simplemente con <x-nombredelcomponente>


Ahora queremos sincronizar la propiedad $search con un input en la vista resources/views/livewire/show-posts.blade.php.
Para poder filtrar nuestra tabla y hacer búsqueda de registros ya sea por title o por content entonces en app/Http/Livewire/ShowPosts.php:
```php
<?php
namespace App\Http\Livewire;
use App\Models\Post;
use Livewire\Component;
class ShowPosts extends Component
{
    public $search;
    public function render()
    {
        $posts = Post::where('title', 'like', '%' . $this->search . '%')
                        ->orWhere('content', 'like', '%' . $this->search . '%')
                        ->get();

        return view('livewire.show-posts', compact('posts'));
    }
}
```
Y en resources/views/livewire/show-posts.blade.php:
```php
<x-table>
  <div class="px-4 py-3">
      <input type="text" wire:model='search'>
  </div>
  ...
```

Para que se vea mas bonito el input box de la caja de search:
https://jetstream.laravel.com/2.x/installation.html
Application Logo:
After installing Jetstream, you may have noticed that the Jetstream logo is utilized on Jetstream's authentication pages as well as your application's top navigation bar. You may easily customize the logo by modifying a few Jetstream components.
Livewire:
If you are using the Livewire stack, you should first publish the Livewire stack's Blade components:
```bash
lando php artisan vendor:publish --tag=jetstream-views
```

Con esto se generan muchos componentes de blade, el que nos interesa usar por el momento es:
resources/views/vendor/jetstream/components/input.blade.php
entonces en en resources/views/livewire/show-posts.blade.php:
```php
<div class="px-4 py-3">
    {{-- <input type="text" wire:model='search'> --}}
    <x-jet-input type="text" wire:model='search' />
</div>
```
Para acceder a los componentes que están dentro de la carpeta resources/views/vendor/jetstream/components/ usar:
```bash
<x-jet-input  /> 
```

Por ultimo si hay algún post despliega la tabla, si no despliega un mensaje, entonces en resources/views/livewire/show-posts.blade.php:
```php
{{-- si hay algún post despliega la tabla --}}
...
@if ($posts->count())
    <table class="w-full">
      ...
    </table>
@else
    <div class="px-4 py-3">
        No existe ningún registro coincidente.
    </div>
@endif
```
Listo!
