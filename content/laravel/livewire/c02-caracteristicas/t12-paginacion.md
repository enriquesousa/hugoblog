---
title: "Paginación"
date: 2023-02-14T12:10:28-08:00
draft: false
weight: 30
menuTitle: "12. Paginación"
---

Se nos olvido la clase pasada usar use WithFileUploads; en app/Http/Livewire/ShowPosts.php para poder subir imágenes!

**ShowPosts**
Ahora vamos a usar el método paginate() en render() en ShowPosts, quitamos el get() y lo sustituimos por paginate(5):
```php
$posts = Post::where('title', 'like', '%' . $this->search . '%')
        ->orWhere('content', 'like', '%' . $this->search . '%')
        ->orderBy($this->sort, $this->direction)
        ->paginate(5);
```

**show-posts**
Y Para colocar los enlaces de Paginación en resources/views/livewire/show-posts.blade.php:
```php
<div class="px-6 py-3">
    {{ $posts->links() }}
</div>
```

**ShowPosts**
Para que la paginacion sea dinamica y no tenga que regargar cada vez que pedimos que cambie de pagina, usamos Livewire\WithPagination en app/Http/Livewire/ShowPosts.php:
```php
use Livewire\WithPagination;

class ShowPosts extends Component
{
    use WithFileUploads;
    use WithPagination;

    ...
}     
```

**show-posts**
Para que los links de Paginación solo estén cuando sea necesario:
```php
{{-- Para mostrar los links de Paginación --}}
@if ($posts->hasPages())
    <div class="px-6 py-3">
        {{ $posts->links() }}
    </div>
@endif 
``` 

En la siguiente clase vamos a resolver el problema de que si nos colocamos en una de las paginas, y hacemos una búsqueda de un registro de otra pagina, nos va a decir que el registro no existe.
Liso!