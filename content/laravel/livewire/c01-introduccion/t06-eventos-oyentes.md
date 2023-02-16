---
title: "Eventos y oyentes"
date: 2023-02-14T12:09:17-08:00
draft: false
weight: 30
menuTitle: "06. Eventos y oyentes"
---

El componente **CreatePost** con su vista **create-post** es independiente del componente **ShowPost**, para poder avisarle al componente **ShowPost** tenemos que avisarle con un evento que vamos a emitir desde el componente **CreatePost**.

Vamos a crear un evento 'renderiza' en app/Http/Livewire/CreatePost.php:
```php
<?php

namespace App\Http\Livewire;

use App\Models\Post;
use Livewire\Component;

class CreatePost extends Component
{
    public $open = false;
    public $title, $content;

    public function render()
    {
        return view('livewire.create-post');
    }

    public function save(){
        // Agregar registro a la tabla posts
        Post::create([
            'title' => $this->title,
            'content' => $this->content,
        ]);

        // Para reset las propiedades de open, title y content
        $this->reset(['open','title','content']);

        $this->emit('renderiza');
    }
}
```

Luego lo escuchamos en app/Http/Livewire/ShowPosts.php:
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

    //Cuando escuches el evento renderiza ejecuta el método render
    protected $listeners = ['renderiza' => 'render'];

    public function render()
    {
        $posts = Post::where('title', 'like', '%' . $this->search . '%')
                        ->orWhere('content', 'like', '%' . $this->search . '%')
                        ->orderBy($this->sort, $this->direction)
                        ->get();

        return view('livewire.show-posts', compact('posts'));
    }

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
Listo!

### Sweetalert
Los eventos también los podemos escuchar desde un scripts, esto nos ayuda para  hacer trigger de eventos, por ejemplo, el plugin de JS sweetalert2:
https://sweetalert2.github.io/

CDN:
<script src="//cdn.jsdelivr.net/npm/sweetalert2@11"></script>

Lo vamos a pegar en el head en la sección de scripts en resources/views/layouts/app.blade.php:
```php
<!-- Scripts -->
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script> 
```
Y para escuchar un scrip en la pagina, tenemos que agregar código JS, hasta abajo  de la pagina, antes de que termine el body, después de @livewireScripts, asi:
```php
<script>
    livewire.on('alerta', function(mensaje){
        Swal.fire(
            'Good job!',
            mensaje,
            'success'
        )
    })
</script>
```

Para que solo un componente escuche el evento, en app/Http/Livewire/CreatePost.php:
```php
$this->emitTo('show-posts','renderiza');
$this->emit('alerta', 'El Post se creó satisfactoriamente');
```
Listo!