---
title: "Acciones en Livewire"
date: 2023-02-14T12:09:01-08:00
draft: false
weight: 20
menuTitle: "04. Acciones en Livewire"
---

Ordenar por ID, title o content.
En app/Http/Livewire/ShowPosts.php:
```php
<?php
namespace App\Http\Livewire;
use App\Models\Post;
use Livewire\Component;
class ShowPosts extends Component
{
    public $search;
    public $sort = 'id';
    public $direction = 'desc';

    public function render()
    {
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

}
```

Pasamos los parametros usando:
```php
wire:click="order('id')"
wire:click="order('title')"
wire:click="order('content')" 
```

En resources/views/livewire/show-posts.blade.php:
```php
<div>

    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Dashboard') }}
        </h2>
    </x-slot>

    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        <!-- component -->
        <x-table>

            <div class="px-4 py-3">
                {{-- <input type="text" wire:model='search'> --}}
                <x-jet-input class="w-full" placeholder="Escriba que quiere buscar" type="text" wire:model='search' />
            </div>

            {{-- si hay algún post despliega la tabla --}}
            @if ($posts->count())
                <table class="w-full">
                    <thead>
                        <tr
                            class="text-md font-semibold tracking-wide text-left text-gray-900 bg-gray-100 uppercase border-b border-gray-600">
                            <th class="cursor-pointer px-4 py-3" wire:click="order('id')">ID</th>
                            <th class="cursor-pointer px-4 py-3" wire:click="order('title')">
                                Titulo
                                <i class="fas fa-sort"></i>
                            </th>
                            <th class="cursor-pointer px-4 py-3" wire:click="order('content')">Contenido</th>
                            <th class="px-4 py-3">Acción</th>
                        </tr>
                    </thead>
                    <tbody class="bg-white">

                        @foreach ($posts as $post)
                        <tr class="text-gray-700">
                            <td class="px-4 py-3 text-ms font-semibold border"> {{ $post->id }} </td>
                            <td class="px-4 py-3 text-ms border"> {{ $post->title }} </td>
                            <td class="px-4 py-3 text-ms border"> {{ $post->content }} </td>
                            <td class="px-4 py-3 text-sm border">Edit</td>
                        </tr>    
                        @endforeach

                    </tbody>
                </table>
            @else
                <div class="px-4 py-3">
                    No existe ningún registro coincidente.
                </div>
            @endif
            
        </x-table>
    </div>

</div>
```

Para adornarlo con fonts de fontawesome usar:
fontawesome-free 
yo tengo la version 4
https://fontawesome.com/versions

Copiar la carpeta que tengo de fontawesome-free que pesa como 3.3MB a: 
- public/vendor/fontawesome-free

Darlo de alta en resources/views/layouts/app.blade.php:
```php
<!-- Styles -->
<link rel="stylesheet" href="{{ mix('css/app.css') }}">
<link rel="stylesheet" href="{{ asset('vendor/fontawesome-free/css/all.min.css') }}">
```

Ya podemos usar sus iconos en resources/views/livewire/show-posts.blade.php con por ejemplo:
<i class="fas fa-sort"></i>.

Donde podemos ver los nombres de los iconos que tenemos disponibles:
https://fontawesome.com/v4/icons/

Al aplicar este icono y acomodarlo al lado derecho que asi en resources/views/livewire/show-posts.blade.php:
### show-posts.blade.php
{{< details "Código" >}}

<div>

    <x-slot name="header">
        <h2 class="font-semibold text-xl text-gray-800 leading-tight">
            {{ __('Dashboard') }}
        </h2>
    </x-slot>

    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-12">
        <!-- component -->
        <x-table>

            <div class="px-4 py-3">
                {{-- <input type="text" wire:model='search'> --}}
                <x-jet-input class="w-full" placeholder="Escriba que quiere buscar" type="text" wire:model='search' />
            </div>

            {{-- si hay algún post despliega la tabla --}}
            @if ($posts->count())
                <table>
                    <thead>
                        
                        <tr class="text-md font-semibold tracking-wide text-left text-gray-900 bg-gray-100 uppercase border-b border-gray-600">
                            <th scope="col" class="cursor-pointer px-4 py-3" wire:click="order('id')">
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
                            <th class="cursor-pointer px-4 py-3" wire:click="order('title')">
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
                            <th scope="col" class="cursor-pointer px-4 py-3" wire:click="order('content')">
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
                            <th scope="col" class="px-4 py-3">Acción</th>
                        </tr>

                    </thead>
                    <tbody class="bg-white">

                        @foreach ($posts as $post)
                        <tr class="text-gray-700">
                            <td class="px-4 py-3 text-ms font-semibold border"> {{ $post->id }} </td>
                            <td class="px-4 py-3 text-ms border"> {{ $post->title }} </td>
                            <td class="px-4 py-3 text-ms border"> {{ $post->content }} </td>
                            <td class="px-4 py-3 text-sm border">Edit</td>
                        </tr>    
                        @endforeach

                    </tbody>
                </table>
            @else
                <div class="px-4 py-3">
                    No existe ningún registro coincidente.
                </div>
            @endif
            
        </x-table>
    </div>

</div>

{{< /details >}}

### Cambiar ancho columna ID
Vamos a cambiar el ancho de la columna ID para que no quede tan pegado el icono de fontawesome al texto ID, solo agregar la clase W-24 para que tenga un ancho fijo, en show-posts.blade.php asi:
```php
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
```

### Listo!
