---
title: "Métodos mágicos y modales"
date: 2023-02-14T12:09:12-08:00
draft: false
weight: 25
menuTitle: "05. Métodos mágicos y modales"
---


### Crear botón para agregar registro:
```php
lando php artisan make:livewire create-post
```
Esto nos crea:
**CLASS:**
app/Http/Livewire/CreatePost.php
**VIEW:**
resources/views/livewire/create-post.blade.php

### Crear Ventana Modal
Crear un modal, uno que ya esta hecho de jet.
resources/views/livewire/create-post.blade.php:
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
            <div class="mb-4">
                <x-jet-label value="Título del Post"></x-jet-label>
                <x-jet-input type="text" class="w-full" wire:model.defer="title"></x-jet-input>
                {{ $title }}
            </div>
            <div class="mb-4">
                <x-jet-label value="Contenido del Post"></x-jet-label>
                <textarea wire:model.defer="content" class="form-control w-full" rows="6"></textarea>
                {{ $content }}
            </div>
        </x-slot>

        <x-slot name='footer'>
            <x-jet-secondary-button class="mr-2" wire:click="$set('open', false)">
                Cancelar
            </x-jet-secondary-button>

            <x-jet-danger-button wire:click="save">
                Crear Post
            </x-jet-danger-button>
        </x-slot>

    </x-jet-dialog-modal>

</div>
```
Para la ventana modal usamos un componente de jetstream:
```php
<x-jet-dialog-modal wire:model='open'> 
```
El cual esta esperando 3 slots! 

**CreatePost.php**
Y en app/Http/Livewire/CreatePost.php:
```php
<?php

namespace App\Http\Livewire;

use App\Models\Post;
use Livewire\Component;

class CreatePost extends Component
{
    public $open = \true;
    public $title, $content;

    public function render()
    {
        return view('livewire.create-post');
    }

    public function save(){
        Post::create([
            'title' => $this->title,
            'content' => $this->content,
        ]);
    }
}
```

### Crear css
Crear resources/css/form.css:
```php
.form-control{
    @apply border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 rounded-md shadow-sm;
}
```
Copiamos los estilos del componente jet input.blade.php
Importar el archivo en resources/css/app.css:
```php
@import 'form.css';

@import 'tailwindcss/base';
@import 'tailwindcss/components';
@import 'tailwindcss/utilities';
```
Ahora compilar.
- lando npm run dev
Hicimos esto para poder usar el input de text area.
Ctrl-F5 en chrome or firefox para refrescar cache!

### Sincronizar variables
Para sincronizar los datos de title y content usamos:
```php
wire:model="title"
wire:model="content" 
```

Para evitar que se comunique el wire:model de nuestro modal al backend cada vez que escribimos un character, usamos en vez de wire:model="title", usamos defer:
```php
wire:model.defer="title"
wire:model.defer="content" 
```
En lo que nos ayuda la propiedad defer, es evitar que haya una comunicación constante entre el componente de livewire (app/Http/Livewire/CreatePost.php) y nuestra vista de livewire (resources/views/livewire/create-post.blade.php).
Con esto evitamos que se este renderizando la vista cada vez que escribimos un caractér en title o content.
Estas propiedades de title y content solo se van actualizar hasta que desencadenemos una acción, en este caso, seria hasta que hagamos click al botón de crear post.

Queda pendiente cerrar la ventana modal y refrescar la pagina principal.
Listo!