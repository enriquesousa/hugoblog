---
title: "Introducción a Livewire"
menuTitle: "01. Introducción a Livewire"
date: 2023-02-14T12:07:35-08:00
draft: false
weight: 5
---

Al crear el proyecto con --jet se incluye en resources/views/layouts/app.blade.php:
```php
<head>
  ...
  @livewireStyles
</head>
<body>
  ...
  @livewireScripts
</body>
```
tanto los livewire Styles como los Scripts.
Los scripts son lo que nos va a permitir darle reactividad a nuestro proyecto. Al colocar estas directivas aquí las podemos usar en todas las vistas que extiendan esta plantilla, como por ejemplo una de las vistas que extiende esta plantilla es resources/views/dashboard.blade.php.

Registrarnos:
```php
Name: Enrique
Email: enrique.sousa@gmail.com
Password: sousa1234
```

Eliminar:
```bash
<div class="bg-white overflow-hidden shadow-xl sm:rounded-lg">
    <x-jet-welcome />
</div>
```
de resources/views/dashboard.blade.php.

Ya podemos trabajar en esta vista.