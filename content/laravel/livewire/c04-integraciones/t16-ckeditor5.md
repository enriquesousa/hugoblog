---
title: "Como integrar CKEditor 5 con Livewire"
date: 2023-02-14T12:10:59-08:00
draft: false
weight: 5
menuTitle: "16. Como integrar CKEditor 5 con Livewire"
---

CKEditor 5 download
https://ckeditor.com/ckeditor-5/download/
 
CDN
```php
<script src="https://cdn.ckeditor.com/ckeditor5/32.0.0/classic/ckeditor.js"></script>
```

El mas actual:
```php
<script src="https://cdn.ckeditor.com/ckeditor5/36.0.1/classic/ckeditor.js"></script> 
```
Blade Stacks
https://laravel.com/docs/10.x/blade#stacks
You may push to a stack as many times as needed. To render the complete stack contents, pass the name of the stack to the @stack directive.

**app.blade.php**
Colocar dos @stack en resources/views/layouts/app.blade.php:
```php
<head>
    ...
    <!-- Styles -->
    @livewireStyles
    @stack('css')
    ...
</head>
<body class="font-sans antialiased">
    ...
    @livewireScripts
    @stack('js')
    ...
</body>
```

**create-post**
Y en resources/views/livewire/create-post.blade.php:
```php
<div wire:init='loadPosts'>
...

    <x-jet-dialog-modal wire:model="open">
        ...
    </x-jet-dialog-modal>

    @push('js')
        <script src="https://cdn.ckeditor.com/ckeditor5/32.0.0/classic/ckeditor.js"></script>

        {{-- inicializar pulgin https://ckeditor.com/docs/ckeditor5/latest/builds/guides/quick-start.html --}}
        <script>
            ClassicEditor
                .create( document.querySelector( '#editor' ) )
                .catch( error => {
                    console.error( error );
                } );
        </script>

    @endpush

</div>
``` 
Todo lo que coloquemos dentro de la directiva @push('js') se coloca en el @stack('js') de app.blade.php.

Usar wire:ignore en el div de textarea para indicar a livewire que no renderize el componente para seguir conservando el estilo del plugin.
Para poder lograr cambiar la propiedad de content tenemos que modificar el js del plugin ckeditor, ya que al momento de user wire:ignore en el textarea perdimos la posibilidad de sincronizar la propiedad content, por esa razón modificaremos el js del puling asi:

**create-post**
en resources/views/livewire/create-post.blade.php:
```php
...
{{-- Contenido del post --}}
<div class="mb-4" wire:ignore>
    <x-jet-label value="Contenido del Post"></x-jet-label>
    <textarea id="editor" 
            wire:model.defer="content" 
            class="form-control w-full" 
            rows="6">

    </textarea>
    <x-jet-input-error for='content' />
</div>
...
<script>
    ClassicEditor
        .create( document.querySelector( '#editor' ) )
        .then(function(editor){
            editor.model.document.on('change:data', () => {
                @this.set('content', editor.getData());    
            })
        })
        .catch( error => {
            console.error( error );
        } );
</script>
...
```
Listo!

Displaying HTML with Blade shows the HTML code
https://stackoverflow.com/questions/29253979/displaying-html-with-blade-shows-the-html-code

para solucionar este problema usar mejor {!! $item->content !!}  en resources/views/livewire/show-posts.blade.php:
```php
<div class="text-sm text-gray-900">
    {{-- {{ $item->content }} --}}
    {!! $item->content !!}
</div>
```
Listo!