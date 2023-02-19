---
title: "Aplazar Carga - Tiempo de Carga de la Tabla"
date: 2023-02-14T12:10:50-08:00
draft: false
weight: 5
menuTitle: "15. Aplazar Carga - Tiempo de Carga de la Tabla"
---

El tiempo de carga nos ayuda para avisar al usuario que ya esta lista nuestra pagina para usar!

**ShowPosts**
Los cambios en app/Http/Livewire/ShowPosts.php:
```php
...
public $readyToLoad = false;
public function render(){
        
    if ($this->readyToLoad) {
        $posts = Post::where('title', 'like', '%' . $this->search . '%')
            ->orWhere('content', 'like', '%' . $this->search . '%')
            ->orderBy($this->sort, $this->direction)
            ->paginate($this->cant);    
    }else{
        $posts = [];
    }

    return view('livewire.show-posts', compact('posts'));
}

...

/*
para indicar a la vista show-posts que ya se cargaron todos los elementos
Esto es posible gracias a <div wire:init='loadPosts'> que tenemos en resources/views/livewire/show-posts.blade.php
*/
public function loadPosts(){
    sleep(2); // para simular el tiempo de carga de una pagina grande! con imágenes.
    $this->readyToLoad = true;
}
```

**show-posts**
Y los cambios en resources/views/livewire/show-posts.blade.php:
```php
// En el main div agregar el método wire:init='loadPosts':
<div wire:init='loadPosts'>

    //En el if de la iteración de los registros modificar la forma en que pasamos la propiedad $posts, y mover los hasPages() dentro del if:
    @if (count($posts))
        <table class="table-auto w-full">
        </table>

        // movimos esto dentro del bloque de @if (count($posts)) después de <table>
        @if ($posts->hasPages())
            <div class="px-6 py-3">
                {{ $posts->links() }}
            </div>    
        @endif
    @else
        @if ($readyToLoad)
            <div class="px-4 py-3">
                No existe ningún registro coincidente.
            </div>
        @else
            <div class="ml-8 mb-4">
                {{-- <i class="fas fa-spinner"></i> --}}
                <p class="text-sm text-red-600">Procesando...</p>
                <div class="w-8 h-8 p-2 bg-red-500 rounded-md animate-spin"></div>
            </div>
        @endif
    @endif

</div>
```

Para el botón "CLEAR" nos limpie variables y reinicie pagina, la redirigimos a dashboard!
**show-posts**
```php
{{-- botón de Clear (limpiar search box) --}}
<button class="bg-indigo-600 px-4 py-2 rounded-md text-white font-semibold tracking-wide cursor-pointer">
    <a href="{{ route('dashboard') }}">
        Clear
    </a>
</button> 
```

Tailwind CSS Animation [Examples](https://larainfo.com/blogs/tailwind-css-animation-examples)
```php
<div class="w-20 h-20 p-2 bg-blue-500 rounded-md animate-spin"></div>
<div class="w-20 h-20 p-2 bg-purple-500 rounded-md animate-ping"></div>
<div class="w-20 h-20 p-2 bg-green-500 rounded-md animate-bounce"></div>
<div class="w-20 h-20 p-2 bg-gray-500 rounded-md animate-pulse"></div> 
Listo!
