---
title: "User Profile Password Change"
date: 2023-05-26T08:49:07-07:00
draft: false    
menuTitle: "41. User Profile Password Change"
weight: 41
---
Cambiar Contraseña
- resources/views/frontend/dashboard/dashboard_sidebar.blade.php
- routes/web.php
- app/Http/Controllers/UserController.php
- resources/views/frontend/dashboard/change_password.blade.php
Listo!
## 42. Update User Login Setup with and without Login
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


