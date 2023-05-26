---
title: "Update Admin Assets Path"
date: 2023-05-25T17:32:23-07:00
draft: false    
menuTitle: "23. Update Admin Assets Path"
weight: 35
---

Mover la carpeta de assets a public/backend/assets y modificar en resources/views/admin/admin_dashboard.blade.php:
```php
<link rel="stylesheet" href="{{ asset('backend/assets/vendors/core/core.css') }}">
y en todas las partes donde sea necesario!
```

Tambien modificar los paths en resources/views/admin/admin_login.blade.php:
```php
<link rel="stylesheet" href="{{ asset('../../../assets/vendors/core/core.css') }}">
a
<link rel="stylesheet" href="{{ asset('backend/assets/vendors/core/core.css') }}">
y en todas las partes donde sea necesario!
```

Volver a cargar la pagina y par alimpiar el cache correr:
- php artisan optimize
