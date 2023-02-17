---
title: "Validaciones"
date: 2023-02-14T12:09:44-08:00
draft: false
weight: 5
menuTitle: "07. Validaciones"
---

- En app/Http/Livewire/CreatePost.php:
```php
 protected $rules = [
    'title' => 'required|max:100',
    'content' => 'required|max:150',
];
 public function save()
{
    $this->validate();

    ...
}
```
- En resources/views/livewire/create-post.blade.php:
```php
@error('title')
    <span>{{ $message }}</span>            
@enderror

y

@error('content')
    <span>{{ $message }}</span>            
@enderror
```
Una mejor manera para mostrar el mensaje de error es usar un componente jet asi:
```php
<x-jet-input-error for='title' />
y
<x-jet-input-error for='content' />
```

Validación en tiempo real
cada vez que se da una letra checa si cumple con las reglas de validación, entonces en app/Http/Livewire/CreatePost.php:
```php
 // se ejecuta cada vez que cambia una de las propiedades title or content
public function updated($propertyName)
{
    // cada vez que se da una letra checa si cumple con las reglas de validación
    $this->validateOnly($propertyName);
}
```
Y tenemos que quitar el defer en resources/views/livewire/create-post.blade.php:
```php
<div class="mb-4">
    <x-jet-label value="Título del Post"></x-jet-label>
    <x-jet-input type="text" class="w-full" wire:model="title"></x-jet-input>
    <x-jet-input-error for='title' />
</div>
```
Listo!