---
title: "Como integrar Sweet Alert 2 con Livewire - botón eliminar un registro"
date: 2023-02-14T12:11:03-08:00
draft: false
weight: 10
menuTitle: "17. Como integrar Sweet Alert 2 con Livewire - botón eliminar un registro"
---

Agregar el botón para poder eliminar un registro.

**Sweetalert2**
https://sweetalert2.github.io/#examples
CDN
```php
<script src="sweetalert2.all.min.js"></script>
```

**show-posts**
Incluirla en nuestro proyecto en resources/views/livewire/show-posts.blade.php:
```php
...
<td class="px-6 py-4 text-sm font-medium flex">
    
    {{-- botón verde Editar --}}
    <a class="btn btn-green" wire:click="edit({{ $item }})">
        <i class="fas fa-edit"></i>
    </a>

    {{-- botón rojo Eliminar --}}
    <a class="btn btn-red ml-2" 
        wire:click="$emit('deletePost', {{ $item->id }})">
        <i class="fas fa-trash"></i>
    </a>

</td>
...
@push('js')

    <script src="sweetalert2.all.min.js"></script>

    <script>
        Livewire.on('deletePost', postId => {
            Swal.fire({
                title: 'Are you sure?',
                text: "You won't be able to revert this!",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#3085d6',
                cancelButtonColor: '#d33',
                confirmButtonText: 'Yes, delete it!'
            }).then((result) => {
                if (result.isConfirmed) {

                    Livewire.emitTo('show-posts', 'delete', postId);

                    Swal.fire(
                    'Deleted!',
                    'Your file has been deleted.',
                    'success'
                    )
                }
            })
        });
    </script>

@endpush
```

**Notas:**
Para que se muestre la alerta solo cuando demos click en el botón rojo de delete ponemos el sweetalert2 dentro de:
```php
Livewire.on('deletePost', postId => {
    ...
});     
```

Para emitir un evento que lo pueda escuchar nuestro componente **ShowPost** mandamos emitir el evento con este código: 
```php
Livewire.emitTo('show-posts', 'delete', postId); 
```
Le pasamos de una vez el ID del post que queremos borrar con:
```php
postId 
```

**ShowPosts**
Para escuchar el nuevo evento 'delete', lo agregamos a los listeners y le agregamos el método 'delete' para que efectivamente borre el post, entonces en app/Http/Livewire/ShowPosts.php:
```php
...
 /*
    Cuando escuches el evento render ejecuta el método render
    protected $listeners = ['render' => 'render'];
    Cuando el nombre de listener es igual al del método podemos omitir uno asi:
    Cuando escuches el evento renderiza ejecuta el método render
*/
protected $listeners = ['renderiza' => 'render', 'delete'];

...
public function delete(Post $post){
    $post->delete();
}
```
Listo!
