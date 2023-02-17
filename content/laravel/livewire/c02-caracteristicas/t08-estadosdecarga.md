---
title: "Estados de carga"
date: 2023-02-14T12:09:56-08:00
draft: false
weight: 10
menuTitle: "08. Estados de carga"
---

Hay varias maneras de avisar al usuario de lso estados de carga.
en resources/views/livewire/create-post.blade.php:
```php
{{-- botón crear post Deshabilitar boton--}}
<x-jet-danger-button wire:click="save" wire:loading.attr="disabled" wire:target='save' class="disabled:opacity-25">
    Crear Post
</x-jet-danger-button>

{{-- botón crear post tambien podemos removerlo --}}
<x-jet-danger-button wire:click="save" wire:loading.remove wire:target='save'>
    Crear Post
</x-jet-danger-button>

{{-- botón crear post tambien podemos cambiar color --}}
<x-jet-danger-button wire:click="save" wire:loading.class="bg-blue-500" wire:target='save'>
    Crear Post
</x-jet-danger-button>

{{-- solo mostrar span cuando se esta ejecutando el método save --}}
{{-- <span wire:loading wire:target='save'>Cargando ...</span> --}}
```
En app/Http/Livewire/CreatePost.php:
```php
public function save()
{
    sleep(1);
    $this->validate();

    // Agregar registro a la tabla posts
    Post::create([
        'title' => $this->title,
        'content' => $this->content,
    ]);

    $this->reset(['open','title','content']);

    $this->emitTo('show-posts','renderiza');
    $this->emit('alerta', 'El Post se creó satisfactoriamente');
}


// se ejecuta cada vez que cambia una de las propiedades title or content
// public function updated($propertyName)
// {
//     // cada vez que se da una letra checa si cumple con las reglas de validación
//     $this->validateOnly($propertyName);
// }
```
Listo!