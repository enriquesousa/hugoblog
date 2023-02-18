---
title: "Subir imágenes"
date: 2023-02-14T12:10:03-08:00
draft: false
weight: 15
menuTitle: "09. Subir imágenes"
---

Agregar campo 'image' a la migración en database/migrations/2022_02_13_172445_create_posts_table.php:
```php
public function up()
{
    Schema::create('posts', function (Blueprint $table) {
        $table->id();

        $table->string('title');
        $table->string('content');
        $table->string('image');

        $table->timestamps();
    });
}
```

**filesystems**
Cambiar el path de acceso de local a public en config/filesystems.php:
```php
'default' => env('FILESYSTEM_DRIVER', 'public'),
```

.**env**
Asegurarnos que también este en .env:
```php
FILESYSTEM_DISK=public 
```

**PostFactory**
Crear imágenes falsas en database/factories/PostFactory.php:
```php
class PostFactory extends Factory
{
    public function definition()
    {
        return [
            'title' => $this->faker->sentence(),
            'content' => $this->faker->text(),
            'image' => 'posts-images/' . $this->faker->image('public/storage/posts-images', 640, 480, null, false), // con false me regresa solo: imagen.jpg
        ];
    }
}
``` 

**DatabaseSeeder**
Crear la carpeta posts-images/ en database/seeders/DatabaseSeeder.php:
```php
<?php

namespace Database\Seeders;

use Illuminate\Database\Seeder;
use Illuminate\Support\Facades\Storage;

class DatabaseSeeder extends Seeder
{
    public function run()
    {
        // Crear la carpeta donde se almacenaran las imágenes
        Storage::deleteDirectory('posts-images');
        Storage::makeDirectory('posts-images');

        \App\Models\Post::factory(100)->create();
    }
}
```
Ahora 
Storage Link con lando!
```bash
lando php artisan storage:link
```

Ahora Ejecutar los Seeders **migrate:fresh**:
```
lando php artisan migrate:fresh --seed
```

Listo!
Volver a registrarnos en el sistema.

Para poder subir y ver imágenes en el componente de livewire en app/Http/Livewire/CreatePost.php:
```php
use Livewire\WithFileUploads;
class CreatePost extends Component
{
    // Para poder usar imágenes en Livewire
    use WithFileUploads;

    public $open = false;
    public $title, $content, $image;
    ...
}
```
Agregamos de una vez la propiedad $image

Crear el input de image y sincronizarla con la propiedad $image
resources/views/livewire/create-post.blade.php
```php
{{-- Imagen del post --}}
<div>
    <input type="file" wire:model="image">

</div> 
```

agregar image a las reglas de validación para que sea requerido, qeu sea una imagen y que máximo tenga 2MB, en app/Http/Livewire/CreatePost.php
```php
protected $rules = [
    'title' => 'required',
    'content' => 'required',
    'image' => 'required|image|max:2048', //image max 2Mb
]; 
```

Alertas de tailwind
https://v1.tailwindcss.com/components/alerts
```php
{{-- tailwind alert --}}
<div class="mb-4 bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative" role="alert">
    <strong class="font-bold">Holy smokes!</strong>
    <span class="block sm:inline">Something seriously bad happened.</span>
    <span class="absolute top-0 bottom-0 right-0 px-4 py-3">
        <svg class="fill-current h-6 w-6 text-red-500" role="button" xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 20 20">
            <title>Close</title>
            <path
                d="M14.348 14.849a1.2 1.2 0 0 1-1.697 0L10 11.819l-2.651 3.029a1.2 1.2 0 1 1-1.697-1.697l2.758-3.15-2.759-3.152a1.2 1.2 0 1 1 1.697-1.697L10 8.183l2.651-3.031a1.2 1.2 0 1 1 1.697 1.697l-2.758 3.152 2.758 3.15a1.2 1.2 0 0 1 0 1.698z" />
        </svg>
    </span>
</div>
```

Agregar el alert en resources/views/livewire/create-post.blade.php:
```php
 <x-slot name='content'>

    {{-- tailwind alert --}}
    <div wire:loading wire:target='image' class="mb-4 bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative" role="alert">
        <strong class="font-bold">Cargando imagen!</strong><span class="block sm:inline">Espere un momento hasta que la imagen se haya procesado.</span>
    </div>

    @if ($image)
        <img class="mb-4" src="{{ $image->temporaryUrl() }}">
        {{ sleep(1) }}
    @endif

    ...
```

Tambien deshabilidar boton de crear post cuando se este cargando una imagen, en resources/views/livewire/create-post.blade.php:
```php
<x-slot name='footer'>
    <x-jet-secondary-button class="mr-2" wire:click="$set('open', false)">
        Cancelar
    </x-jet-secondary-button>

    <x-jet-danger-button wire:click="save" wire:loading.attr="disabled" wire:target='save, image'
        class="disabled:opacity-25">
        Crear Post
    </x-jet-danger-button>
</x-slot>
```
Listo!

**CreatePost**
Para salvar la imagen.
Código completo en app/Http/Livewire/CreatePost.php:
```php
<?php

namespace App\Http\Livewire;

use App\Models\Post;
use Livewire\Component;
use Livewire\WithFileUploads;

class CreatePost extends Component
{
    use WithFileUploads;

    public $open = true;
    public $title, $content, $image, $identificador;

    protected $rules = [
        'title' => 'required',
        'content' => 'required',
        'image' => 'required|image|max:2048', //image max 2Mb
    ];

    public function render(){
        return view('livewire.create-post');
    }

    public function mount(){
        $this->identificador = rand(); //init con numero al azar
    }

    public function save(){
        sleep(1);
        $this->validate();

        // guardar la imagen en carpeta public/posts
        // $image_url = $this->image->store('posts');
        // guardar la imagen en carpeta public/posts-images
        // $image_url = $this->image->store('public/posts-images');

        // guardar la imagen en carpeta storage/app/public/posts-images/
        $image_url = $this->image->store('posts-images');


        // Agregar registro a la tabla posts
        Post::create([
            'title' => $this->title,
            'content' => $this->content,
            'image' => $image_url,
        ]);

        $this->reset(['open','title','content','image']);
        $this->identificador = rand(); //init con numero al azar

        $this->emitTo('show-posts','renderiza');
        $this->emit('alerta', 'El Post se creó satisfactoriamente');
    }


    // se ejecuta cada vez que cambia una de las propiedades title or content
    // public function updated($propertyName)
    // {
    //     // cada vez que se da una letra checa si cumple con las reglas de validación
    //     $this->validateOnly($propertyName);
    // }

}
```
En app/Models/Post.php:
```php
protected $fillable = [
    'title',
    'content',
    'image',
];
```

**create-post**
Código completo en resources/views/livewire/create-post.blade.php:
```php
<div>
    {{-- Do your work, then step back. --}}
    <x-jet-danger-button wire:click="$set('open', true)">
        Crear post
    </x-jet-danger-button>

    <x-jet-dialog-modal wire:model='open'>

        <x-slot name='title'>
            Crear nuevo post
        </x-slot>

        <x-slot name='content'>

            {{-- tailwind alert, para avisar al user que se esta subiendo una imagen --}}
            <div wire:loading wire:target='image' class="mb-4 bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative" role="alert">
                <strong class="font-bold">¡Cargando imagen!</strong>
                <span class="sm:inline">Espere un momento ...</span>
                {{-- con block lo baja una linea --}}
                {{-- <span class="block sm:inline">Espere un momento, en lo que procesa ...</span>  --}}
            </div>

            {{-- Cada vez que se selecciona una imagen en el input de input type="file", se almacena una imagen temporal en temporaryUrl() que se encuentra real en path /storage/app/livewire-tmp  --}}
            @if ($image)
                <img class="mb-4" src="{{ $image->temporaryUrl() }}">
                {{-- use sleep(1) un segundo para simular tiempo de espera de internet --}}
                {{ sleep(1) }} 
            @endif

            {{-- Titulo del post --}}
            <div class="mb-4">
                <x-jet-label value="Título del Post"></x-jet-label>
                <x-jet-input type="text" class="w-full" wire:model="title"></x-jet-input>
                <x-jet-input-error for='title' />
            </div>

            {{-- Contenido del post --}}
            <div class="mb-4">
                <x-jet-label value="Contenido del Post"></x-jet-label>
                <textarea wire:model.defer="content" class="form-control w-full" rows="6"></textarea>
                <x-jet-input-error for='content' />
            </div>


            {{-- imagen del post --}}
            <div>
                <input type="file" wire:model='image' id="{{ $identificador }}">
                <x-jet-input-error for='image' />
            </div>
             {{-- Cualquier imagen que seleccionemos en este input sera almacenada en la propiedad image, gracias al wire:model='image' --}}

        </x-slot>

        <x-slot name='footer'>
            <x-jet-secondary-button class="mr-2" wire:click="$set('open', false)">
                Cancelar
            </x-jet-secondary-button>

            // deshabilita botón "Crear Post" cuando se este salvando y/o subiendo imagen
            <x-jet-danger-button wire:click="save" wire:loading.attr="disabled" wire:target='save, image'
                class="disabled:opacity-25">
                Crear Post
            </x-jet-danger-button>

            {{-- solo mostrar span cuando se esta ejecutando el método save --}}
            {{-- <span wire:loading wire:target='save'>Cargando ...</span> --}}
        </x-slot>

    </x-jet-dialog-modal>

</div>
```
Nota:
Deshabilita botón "Crear Post" cuando se este salvando y/o subiendo imagen.
Esto lo logramos con:
```php
wire:target='save, image' 
```

No olvidar dar de Alta image a la asignación masiva en:
app/Models/Post.php
```php
protected $fillable = ['title', 'content', 'image']; 
```
Listo!

usar $identificador para que haga reset a la variable file del input de la imagen y asi ya no ver el ultimo nombre de archivo que subimos.
Listo!