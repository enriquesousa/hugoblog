---
title: "Frontend Template Setup Part 1"
date: 2023-05-26T08:46:54-07:00
draft: false    
menuTitle: "32. Frontend Template Setup Part 1"
weight: 32
---
Cargar un tema para el frontend para todo los usuarios.
Copiar el Frontend theme de:
~/Sites/recursos/udemy/Laravel 10 - Build Real Estate Property Listing Project A-Z/Course+Excise+Files/Course Excise Files/Frontend

Crear nuevo controlador
```php
php artisan make:controller UserController
```

Copiar los assets de: ~/Sites/recursos/udemy/Laravel 10 - Build Real Estate Property Listing Project A-Z/Course+Excise+Files/Course Excise Files/Frontend/assets
a: public/frontend/assets

y copiar el index a:
resources/views/frontend/frontend_dashboard.blade.php

Update loas accesos a css js e images, en resources/views/frontend/frontend_dashboard.blade.php:
usando: {{ asset('frontend/') }}
```php
...
<!-- Stylesheets -->
<link href="{{ asset('frontend/assets/css/font-awesome-all.css') }}" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/flaticon.css') }}" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/owl.css') }}" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/bootstrap.css') }}" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/jquery.fancybox.min.css') }}" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/animate.css') }}" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/jquery-ui.css') }}" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/nice-select.css') }}" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/color/theme-color.css') }}" id="jssDefault" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/switcher-style.css') }}" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/style.css') }}" rel="stylesheet">
<link href="{{ asset('frontend/assets/css/responsive.css') }}" rel="stylesheet">
...
<!-- jequery plugins -->
<script src="{{ asset('frontend/assets/js/jquery.js') }}"></script>
<script src="{{ asset('frontend/assets/js/popper.min.js') }}"></script>
<script src="{{ asset('frontend/assets/js/bootstrap.min.js') }}"></script>
<script src="{{ asset('frontend/assets/js/owl.js') }}"></script>
<script src="{{ asset('frontend/assets/js/wow.js') }}"></script>
<script src="{{ asset('frontend/assets/js/validation.js') }}"></script>
<script src="{{ asset('frontend/assets/js/jquery.fancybox.js') }}"></script>
<script src="{{ asset('frontend/assets/js/appear.js') }}"></script>
<script src="{{ asset('frontend/assets/js/scrollbar.js') }}"></script>
<script src="{{ asset('frontend/assets/js/isotope.js') }}"></script>
<script src="{{ asset('frontend/assets/js/jquery.nice-select.min.js') }}"></script>
<script src="{{ asset('frontend/assets/js/jQuery.style.switcher.min.js') }}"></script>
<script src="{{ asset('frontend/assets/js/jquery-ui.js') }}"></script>
<script src="{{ asset('frontend/assets/js/nav-tool.js') }}"></script> 
```

En routes/web.php
```php
// Home User Frontend All Route
Route::get('/', [UserController::class, 'index']); 
```

Y en app/Http/Controllers/UserController.php:
```php
public function index(){
    return view('frontend.frontend_dashboard');
} 
```
Antes de probar, No olvidar correr:
```php
php artisan optimize 
```
No funciono!

Ya funciono!
me estaba faltando el asset:
```php 
<!-- main-js -->
<script src="{{ asset('frontend/assets/js/script.js') }}"></script>
```

También actualice todos las {{ asset('frontend/) }} para todas las images de la pagina, que son como 50!
Listo!

