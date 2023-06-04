---
title: "Update User Login Setup with and without Login"
date: 2023-05-26T08:49:19-07:00
draft: false    
menuTitle: "42. Update User Login Setup with and without Login"
weight: 42
---
En resources/views/frontend/home/header.blade.php
```php
@auth
    <div class="sign-box">
        <a href="{{ route('dashboard') }}"><i class="fas fa-user"></i>Panel</a>
        <a href="{{ route('user.logout') }}" class="pl-3"><i class="fas fa-user"></i>Cerrar Sesión</a>
    </div>
@else
    <div class="sign-box">
        <a href="{{ route('login') }}"><i class="fas fa-user"></i>Iniciar Sesión</a>
    </div>
@endauth 
```
Listo!

