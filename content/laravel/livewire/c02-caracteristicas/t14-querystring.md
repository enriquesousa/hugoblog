---
title: "Query String - datos por la url"
date: 2023-02-14T12:10:36-08:00
draft: false
weight: 40
menuTitle: "14. Query String - datos por la url"
---

**show-posts**
Para mostrar una cantidad por pagina, podemos hacer que el usuario escoja de una lista de opciones, para hacer esto añadir este código en  resources/views/livewire/show-posts.blade.php:
```php
{{-- Escoger cuantas paginas deseamos ver --}}
<div class="flex items-center">
    <span>Mostrar</span>
    <select wire:model="cant" class="mx-2 form-control">
        <option value="5">5</option>
        <option value="10">10</option>
        <option value="50">50</option>
        <option value="100">100</option>
    </select>
    <span>entradas</span>
</div> 
``` 

**ShowPosts**
Y sincronizar con la propiedad $cant en app/Http/Livewire/ShowPosts.php:
```php

    public $cant = '10';

    public function render()
    {
        // $posts = Post::all();
        $posts = Post::where('title', 'like', '%' . $this->search . '%')
                        ->orWhere('content', 'like', '%' . $this->search . '%')
                        ->orderBy($this->sort, $this->direction)
                        ->paginate($this->cant);

        return view('livewire.show-posts', compact('posts'));
    }
```

**ShowPosts**
Para poder pasar el parámetro de la cantidad por la url hacemos, a esto es lo que se le llama Query String:
```php
public $search = '';
public $post, $image, $identificador;
public $sort = 'id';
public $direction = 'desc';
public $open_edit = false;
public $cant='10';

// Que info queremos que viaje por la url, 'except' nos ayuda a no colocar ciertos valores en la url
protected $queryString = [
    'cant' => ['except' => '10'],
    'sort' => ['except' => 'id'],
    'direction' => ['except' => 'desc'],
    'search' => ['except' => '']
]; 
``` 
La razón por la cual convertimos en una cadena a $cant, fue para no tener problemas cuando veníamos de escoger otra cantidad y regresábamos a 10, seguíamos viendo el 10 en los parámetros de la url, y esto era porque la url nos regresaba una cadena en vez de un dato numérico. Al hacerlo cadena resolvimos el problema!
No afecto a  wire:model="cant" ni en ->paginate($this->cant) ya que es lo suficientemente inteligente para saber que estamos hablando de un valor.
Listo!