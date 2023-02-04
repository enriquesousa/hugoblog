---
title: "Enrutamiento"
menuTitle: "21. Enrutamiento"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 5
---

### Como define Voyager sus rutas
Vemos como Voyager define sus grupo de rutas en routes/web.php
```php
Route::group(['prefix' => 'admin'], function () {
    Voyager::routes();
}); 
``` 
Cambiar el prefijo de 'admin' por 'administrador' 
```php
Route::group(['prefix' => 'administrador'], function () {
    Voyager::routes();
}); 
``` 
Con este cambio ahora ya podemos visitar:
http://voyager.lndo.site/administrador
http://voyager.lndo.site/administrador/products
En lugar de usar 'admin'

Nota:
Volver a dejarlo en 'admin'
