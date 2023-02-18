---
title: "Pasar parámetros de acción"
date: 2023-02-14T12:10:22-08:00
draft: false
weight: 25
menuTitle: "11. Pasar parámetros de acción"
---

Ya NO vamos a usar el componente de anidamiento:
```php
{{-- @livewire('edit-post', ['post' => $post], key($post->id)) --}} 
```
En resources/views/livewire/show-posts.blade.php
Porque esto hace mas lento la aplicación, lo mejor seria usar un solo modal para toda la pagina de show-posts y en vez de componentes anidados que crean una modal cada vez que se manda llamar, mejor usamos botones que nos van a mandar llamar a la única ventana modal de la vista.

Código completo!
**ShowPosts**
En app/Http/Livewire/ShowPosts.php:
```php
<?php

namespace App\Http\Livewire;

use Livewire\Component;
use App\Models\Post;
use Illuminate\Support\Facades\Storage;
use Livewire\WithFileUploads;

class ShowPosts extends Component
{
    use WithFileUploads;

    public $search, $post, $image, $identificador;
    public $sort = 'id';
    public $direction = 'desc';

    public $open_edit = false;

    public function mount(){
        $this->identificador = rand();

        // esto es para que la variable $image se convierta ya en un objeto
        // el cual la estaremos usando en el modal de resources/views/livewire/show-posts.blade.php
        $this->post = new Post();
    }

    protected $rules = [
        'post.title' => 'required',
        'post.content' => 'required',
    ];

    // Cuando escuches el evento render ejecuta el método render
    // protected $listeners = ['render' => 'render'];
    // Cuando el nombre de listener es igual al del método podemos omitir uno asi:
    //Cuando escuches el evento renderiza ejecuta el método render
    protected $listeners = ['renderiza' => 'render'];

    public function render()
    {
        // $posts = Post::all();
        $posts = Post::where('title', 'like', '%' . $this->search . '%')
                        ->orWhere('content', 'like', '%' . $this->search . '%')
                        ->orderBy($this->sort, $this->direction)
                        ->get();

        return view('livewire.show-posts', compact('posts'));
    }

    // Recibimos parámetro que nos envía la vista show-posts en var $sort
    public function order($sort)
    {
        if ($this->sort == $sort) {
            if ($this->direction == 'desc') {
                $this->direction = 'asc';
            } else {
                $this->direction = 'desc';
            }
        } else {
            $this->sort = $sort;
            $this->direction = 'asc';
        }
    }

    public function edit(Post $post){
        $this->post = $post;
        $this->open_edit = true;
    }

    public function update(){
        $this->validate();

        //check si escogieron una imagen, eliminamos imagen almacenada
        if ($this->image) {
            // eliminamos imagen almacenada
            Storage::delete([$this->post->image]);
            // subir nueva imagen
            $this->post->image = $this->image->store('public/posts-images');
        }

        $this->post->save();
        $this->reset(['open_edit', 'image']);
        $this->identificador = rand();
        $this->emit('alerta', 'El Post se actualizó satisfactoriamente');
    }

}
```

Código completo!
**show-posts**
En resources/views/livewire/show-posts.blade.php:
```php
<div>
    {{-- Vista Mostrar todos los Post --}}
    {{-- If your happiness depends on money, you will never be happy with yourself. --}}

    {{-- header dashboard --}}
    {{-- <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Dashboard') }}
        </h2>
    </x-slot> --}}

    {{-- componente table livewire --}}
    <x-table>

        {{-- Header General - Titulo, Búsqueda y Botones --}}
        <div class="flex items-center justify-between pb-6">

            {{-- Titulo de la tabla Justificada a la Izquierda --}}
            <div>
                <h2 class="text-gray-600 font-semibold">Productos</h2>
                <span class="text-xs">All products item</span>
            </div>

            {{-- Rectángulo de búsqueda (search) --}}
            <div class="flex bg-gray-50 items-center p-2 rounded-md space-x-2">

                {{-- icono lupa de search --}}
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" viewBox="0 0 20 20" fill="currentColor">
                    <path fill-rule="evenodd" d="M8 4a4 4 0 100 8 4 4 0 000-8zM2 8a6 6 0 1110.89 3.476l4.817 4.817a1 1 0 01-1.414 1.414l-4.816-4.816A6 6 0 012 8z" clip-rule="evenodd" />
                </svg>

                {{-- rectángulo de input para escribir lo que buscamos --}}
                {{-- <input class="bg-gray-50 outline-none ml-0 block" type="text" name="" id="" placeholder="search..." wire:model='search'> --}}
                {{-- usar botón de jetstream --}}
                <x-jet-input type="text" placeholder="buscar..." wire:model='search' />

                {{-- botón de Clear (limpiar search box) --}}
                <button class="bg-indigo-600 px-4 py-2 rounded-md text-white font-semibold tracking-wide cursor-pointer">
                    <a href="{{ $search='' }}">
                        Clear
                    </a>
                </button>
            </div>

            {{-- Rectángulo de búsqueda (search) y Botones a la Derecha New y Create --}}
            <div class="flex items-center justify-between">

                {{-- Botones a la Derecha New y Create --}}
                {{-- <div class="lg:ml-40 ml-10 space-x-8">
                    <button class="bg-indigo-600 px-4 py-2 rounded-md text-white font-semibold tracking-wide cursor-pointer">
                        Create
                    </button>
                </div> --}}

                @livewire('create-post')

            </div>

        </div>

        @if ($posts->count())

            {{-- Tabla --}}
            <table class="min-w-full leading-normal">

                {{-- Header de la Tabla --}}
                <thead>
                    <tr>
                        <th class="w-24 cursor-pointer px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider" wire:click="order('id')">
                            ID

                            {{-- sort --}}
                            @if ($sort == 'id')
                                @if ($direction == 'asc')
                                    <i class="fas fa-sort-numeric-down float-right mt-1"></i>
                                @else
                                    <i class="fas fa-sort-numeric-up float-right mt-1"></i>
                                @endif
                            @else
                                <i class="fa fa-sort float-right mt-1"></i>
                            @endif

                        </th>
                        <th class="cursor-pointer px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider" wire:click="order('title')">
                            Titulo

                            {{-- sort --}}
                            @if ($sort == 'title')
                                @if ($direction == 'asc')
                                    <i class="fas fa-sort-alpha-down float-right mt-1"></i>
                                @else
                                    <i class="fas fa-sort-alpha-up float-right mt-1"></i>
                                @endif
                            @else
                                <i class="fa fa-sort float-right mt-1"></i>
                            @endif

                        </th>
                        <th class="cursor-pointer px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider" wire:click="order('content')">
                            Contenido

                            {{-- sort --}}
                            @if ($sort == 'content')
                                @if ($direction == 'asc')
                                    <i class="fas fa-sort-alpha-down float-right mt-1"></i>
                                @else
                                    <i class="fas fa-sort-alpha-up float-right mt-1"></i>
                                @endif
                            @else
                                <i class="fa fa-sort float-right mt-1"></i>
                            @endif

                        </th>
                        <th class="px-5 py-3 border-b-2 border-gray-200 bg-gray-100 text-left text-xs font-semibold text-gray-600 uppercase tracking-wider">
                            Acción
                        </th>
                    </tr>
                </thead>

                {{-- Cuerpo de la Tabla --}}
                <tbody>
                    @foreach ($posts as $item)
                        {{-- Renglón --}}
                        <tr>

                            {{-- Columna 1 - ID --}}
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">
                                    {{ $item->id }}
                                </p>
                            </td>

                            {{-- Columna 2 - Titulo --}}
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">
                                    {{ $item->title }}
                                </p>
                            </td>

                            {{-- Columna 3 - Contenido --}}
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                <p class="text-gray-900 whitespace-no-wrap">
                                    {{ $item->content }}
                                </p>
                            </td>

                            {{-- Columna 4 - Acción --}}
                            <td class="px-5 py-5 border-b border-gray-200 bg-white text-sm">
                                {{-- @livewire('edit-post', ['post' => $post], key($post->id)) --}}
                                <a class="btn btn-green" wire:click="edit({{ $item }})">
                                    <i class="fas fa-edit"></i>
                                </a>
                            </td>

                        </tr>
                    @endforeach
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

        @else

            <div class="px-4 py-3">
                No existe ningún registro coincidente.
            </div>

        @endif

    </x-table>

    {{-- Ventana modal para editar un post --}}
    <x-jet-dialog-modal wire:model="open_edit">

        <x-slot name='title'>
            {{-- Editar el post - {{ $post->title }} --}}
            Editar el post
        </x-slot>

        <x-slot name='content'>

            {{-- tailwind alert, para avisar al user que se esta subiendo una imagen --}}
            <div wire:loading wire:target='image' class="mb-4 bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded relative" role="alert">
                <strong class="font-bold">Cargando imagen!</strong>
                <span class="sm:inline">Espere un momento ...</span>
            </div>

            @if ($image)
                <img class="mb-4" src="{{ $image->temporaryUrl() }}">
                {{ sleep(1) }}
            @else
                <img src="{{ Storage::url($post->image) }}">
            @endif

            {{-- Título del post --}}
            <div class="mb-4">
                <x-jet-label value="Título del post" />
                <x-jet-input wire:model="post.title" type="text" class="w-full" />
            </div>

            {{-- Contenido del post --}}
            <div>
                <x-jet-label value="Contenido del post" />
                <textarea wire:model="post.content" rows="6" class="form-control w-full mb-4"></textarea>
            </div>

            {{-- imagen --}}
            <div>
                <x-jet-label value="Escoja imagen para subir" />
                <input type="file" wire:model='image' id="{{ $identificador }}">
                <x-jet-input-error for='image' />
            </div>

        </x-slot>

        <x-slot name='footer'>
            <x-jet-secondary-button wire:click="$set('open_edit', false)">
                Cancelar
            </x-jet-secondary-button>

            <x-jet-danger-button wire:click="update" wire:loading.attr='disabled' class="ml-2 disabled:opacity-25">
                Actualizar
            </x-jet-danger-button>
        </x-slot>

    </x-jet-dialog-modal>

</div>
``` 
Listo!