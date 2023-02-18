---
title: "Ciclos de vida - updatingSearch()"
date: 2023-02-14T12:10:32-08:00
draft: false
weight: 35
menuTitle: "13. Ciclos de vida - updatingSearch()"
---

Hasta ahorita hemos visto solo el ciclo de vida de mount(), pero para resolver el problema en que nos quedamos en la clase pasada, ahora vamos a conocer otros tipos de componentes que nos ofrece livewire:

Lifecycle Hooks
https://laravel-livewire.com/docs/2.x/lifecycle-hooks

El que nos puede ayudar es:
```
updatingFoo()
```
> Runs before a property called $foo is updated. Array properties have an additional $key argument passed to this function to specify changing element inside array, like updatingArray($value, $key)

Entonces en app/Http/Livewire/ShowPosts.php:
```php
public function updatingSearch(){
    // Este método se va a ejecutar cada vez que se haga un cambio a la propiedad $search
    // Y resetPage() nos ayuda a quitar la Paginación del componente para poder encontrar el registro 
    $this->resetPage();
}
```
Nota como cambiamos la primera letra de la propiedad $search por Mayúscula 'S'.

Listo!