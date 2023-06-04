---
title: "User Profile Design Part 3"
date: 2023-05-26T08:48:48-07:00
draft: false    
menuTitle: "39. User Profile Design Part 3"
weight: 39
---
Update User
Archivos que modifique:
- app/Http/Controllers/UserController.php
- resources/views/frontend/frontend_dashboard.blade.php
- resources/views/frontend/dashboard/edit_profile.blade.php
- resources/views/admin/admin_profile_view.blade.php
- routes/web.php

Nota:
En app/Http/Controllers/UserController.php
Para que no falle la linea:
unlink(public_path('upload/user_images/' . $data->photo)); // para borrar la imagen anterior
La primera vez tenemos que comentarla, para salvar por lo menos una imagen.
Ya que este salvada la primer imagen ya podemos des-comentarla.

Para arreglar este problema usar:
```php
// dd($data->photo); //regresa null si es la primera vez (si no hay foto)
if (!empty($data->photo)) {
    unlink(public_path('upload/user_images/' . $data->photo)); // para borrar la imagen anterior
}
```
Listo!

