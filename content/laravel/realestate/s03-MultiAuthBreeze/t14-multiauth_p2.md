---
title: "Multi Auth Breeze Part 2"
date: 2023-05-25T17:30:06-07:00
draft: false
menuTitle: "14. Multi Auth Breeze Part2"
weight: 35
---

- Para ver la lista de routes:
php artisan r:l

- Controlador responsable de redirigir:
app/Http/Controllers/Auth/AuthenticatedSessionController.php

- El control de router esta en:
app/Providers/RouteServiceProvider.php

Entonces para poder redirigir al usuario a nuestras tres rutas posibles:
- user
- admin
- agent

Tenemos que trabajar en:
app/Http/Controllers/Auth/AuthenticatedSessionController.php

Los datos los tomamos del Login Request:
app/Http/Requests/Auth/LoginRequest.php

En app/Http/Controllers/Auth/AuthenticatedSessionController.php 
Entonces podemos agregar el codigo de comparacion de que rol tiene el usuario asi:
```php
$url = '';
if ($request->user()->role === 'admin') {
    $url = 'admin/dashboard';
}elseif($request->user()->role === 'agent'){
    $url = 'agent/dashboard';
}elseif($request->user()->role === 'user'){
    $url = '/dashboard';
}

return redirect()->intended($url);
```
Listo!
Hasta aqui la redireccion esta lista, ahora falta la proteccion de las rutas para que un 
rol de user no pueda entrar a los otros dos dashboards (admin y agent).