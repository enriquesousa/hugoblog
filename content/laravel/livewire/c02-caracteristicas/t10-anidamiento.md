---
title: "Componentes de anidamiento EditPost"
date: 2023-02-14T12:10:08-08:00
draft: false
weight: 20
menuTitle: "10. Componentes de anidamiento EditPost"
---

Crear componente livewire
```bash
lando php artisan make:livewire EditPost 
```
CLASS: app/Http/Livewire/EditPost.php
VIEW:  resources/views/livewire/edit-post.blade.php

Código css para asignarle de manera fácil la característica de un botón a lo iconos de fontawesome que vamos a usar para edit,borrar etc.  

**buttons.css**
Para asignarle los estilos de tailwind a los botones: 
Crear resources/css/buttons.css:
```php
.btn{
    @apply font-bold text-white py-2 px-4 rounded cursor-pointer;
}

.btn-red{
    @apply bg-red-600 hover:bg-red-500;
}

.btn-blue{
    @apply bg-blue-600 hover:bg-blue-500;
}

.btn-green{
    @apply bg-green-600 hover:bg-green-500;
}
```

**app.css**
Importar en resources/css/app.css:
```php
@import 'tailwindcss/base';
@import 'tailwindcss/components';
@import 'tailwindcss/utilities';

@import 'form.css';
@import 'buttons.css';
```

Compilar los estilos con:
En **Mix**
```bash
lando npm run dev

Dejar corriendo con:
lando npm run watch
```
En **Vite**
```bash
lando dev
```

**edit-post**
resources/views/livewire/edit-post.blade.php:
```php
<div>

    <a class="btn btn-green" wire:click="$set('open', true)">
        <i class="fas fa-edit"></i>
    </a>

    <x-jet-dialog-modal wire:model="open">

        <x-slot name='title'>
            Editar el post {{ $post->title }}
        </x-slot>

        <x-slot name='content'>
        </x-slot>

        <x-slot name='footer'>
        </x-slot>

    </x-jet-dialog-modal>

</div>
```
Listo!

Queremos que cuando haga click en el botón, se abra el modal de:
```php
<x-jet-dialog-modal> 
```

**edit-post** Ventana modal
En resources/views/livewire/edit-post.blade.php:
```php
<div>

    <a class="btn btn-green" wire:click="$set('open', true)">
        <i class="fas fa-edit"></i>
    </a>

    <x-jet-dialog-modal wire:model="open">

        <x-slot name='title'>
            {{-- Editar el post - {{ $post->title }} --}}
            Editar el post
        </x-slot>

        <x-slot name='content'>
            <div class="mb-4">
                <x-jet-label value="Título del post" />
                <x-jet-input wire:model="post.title" type="text" class="w-full" />
            </div>
            <div>
                <x-jet-label value="Contenido del post" />
                <textarea wire:model="post.content" rows="6" class="form-control w-full"></textarea>
            </div>
        </x-slot>

        <x-slot name='footer'>
            <x-jet-secondary-button wire:click="$set('open', false)">
                Cancelar
            </x-jet-secondary-button>

            <x-jet-danger-button wire:click="save" wire:loading.attr='disabled' class="ml-2 disabled:opacity-25">
                Actualizar
            </x-jet-danger-button>
        </x-slot>

    </x-jet-dialog-modal>

</div>
```
wire:model="open" para sincronizar con la propiedad 
public $open = false;

**EditPost**
En app/Http/Livewire/EditPost.php:
```php
<?php

namespace App\Http\Livewire;

use App\Models\Post;
use Livewire\Component;

class EditPost extends Component
{
    public $open = false;
    public $post;

    protected $rules = [
        'post.title' => 'required',
        'post.content' => 'required',
    ];

    public function mount(Post $post){
        $this->post = $post;
    }

    public function save(){
        $this->validate();
        $this->post->save();
        $this->reset(['open']);
        $this->emitTo('show-posts','renderiza');
        $this->emit('alerta', 'El Post se actualizó satisfactoriamente');
    }

    public function render(){
        return view('livewire.edit-post');
    }
}
```
En mount() recibimos la propiedad que le enviamos ($post), que se trata de una instancia del modelo Post!, una vez que lo recibimos se lo asignamos a la propiedad $this->post = $post.
Listo!

Ahora agregar un selector de archivos para seleccionar una imagen también igual que lo hicimos en crear post!

**edit-post**
En resources/views/livewire/edit-post.blade.php:
```php
<div>

    {{-- Fontawesome icono de Edit --}}
    <a class="btn btn-green" wire:click="$set('open', true)">
        <i class="fas fa-edit"></i>
    </a>

    {{-- Ventana Modal, contiene 3 slots --}}
    <x-jet-dialog-modal wire:model="open">

        {{-- Titulo  --}}
        <x-slot name='title'>
            {{-- Editar el post - {{ $post->title }} --}}
            Editar el post
        </x-slot>

        {{-- Contenido --}}
        <x-slot name='content'>

            {{-- tailwind alert --}}
            <div wire:loading wire:target='image' class="mb-4 bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative" role="alert">
                <strong class="font-bold">Cargando imagen!</strong>
                <span class="sm:inline">Espere un momento ...</span>
            </div>

            {{-- Mostrar imagen a editar --}}
            @if ($image)
                <img class="mb-4" src="{{ $image->temporaryUrl() }}">
                {{ sleep(1) }}
            @else
                <img src="{{ Storage::url($post->image) }}">
            @endif

            {{-- Titulo del Post --}}
            <div class="mb-4">
                <x-jet-label value="Título del post" />
                <x-jet-input wire:model="post.title" type="text" class="w-full" />
            </div>

            {{-- Contenido del Post --}}
            <div>
                <x-jet-label value="Contenido del post" />
                <textarea wire:model="post.content" rows="6" class="form-control w-full mb-2"></textarea>
            </div>

            {{-- imagen --}}
            <div>
                <x-jet-label value="Escoja una imagen" />
                <input type="file" wire:model='image' id="{{ $identificador }}">
                <x-jet-input-error for='image' />
            </div>

        </x-slot>

        {{-- Footer  --}}
        <x-slot name='footer'>
            <x-jet-secondary-button wire:click="$set('open', false)">
                Cancelar
            </x-jet-secondary-button>

            <x-jet-danger-button wire:click="save" wire:loading.attr='disabled' class="ml-2 disabled:opacity-25">
                Actualizar
            </x-jet-danger-button>
        </x-slot>

    </x-jet-dialog-modal>

</div>
```

Para poder ver las propiedades en lo campos de titulo y contenido de la ventana modal que acabamos de programar, necesitamos definir las propiedades como reglas asi, en EditPost.php, al definir nuestras reglas de validación, nos permite utilizar directamente las propiedades del objeto:
```php
protected $rules = [
    'post.title' => 'required',
    'post.content' => 'required',
]; 
```

**EditPost** Código completo.
En app/Http/Livewire/EditPost.php:
```php
<?php

namespace App\Http\Livewire;

use App\Models\Post;
use Livewire\Component;
use Livewire\WithFileUploads;
use Illuminate\Support\Facades\Storage;

class EditPost extends Component
{
    use WithFileUploads;

    public $open = false;
    public $post;
    public $image, $identificador;

    protected $rules = [
        'post.title' => 'required',
        'post.content' => 'required',
    ];

    public function mount(Post $post){
        $this->post = $post;
        $this->identificador = rand(); //init con numero al azar
    }


    public function save(){
        $this->validate();

        //check si escogieron una imagen, eliminamos imagen almacenada
        if ($this->image) {
            // eliminamos imagen almacenada asociada a este post
            Storage::delete([$this->post->image]);
            // subir nueva imagen
            // $this->post->image = $this->image->store('posts-images');
            $this->post->image = $this->image->store('posts-images');
        }

        $this->post->save();
        $this->reset(['open', 'image']);
        $this->identificador = rand();
        $this->emitTo('show-posts','renderiza');
        $this->emit('alerta', 'El Post se actualizó satisfactoriamente');
    }

    public function render()
    {
        return view('livewire.edit-post');
    }
}
```
Listo!