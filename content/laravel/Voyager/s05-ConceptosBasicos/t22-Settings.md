---
title: "Settings"
menuTitle: "22. Settings"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 10
---

### Cambiar en forma sencilla logo y titulo
Modificar en Settings.
![Widgets](/Voyager/settingslogo.png)

Para insertar esto parámetros del nuevo logo y titulo hacerlo asi, por ejemplo en resources/views/welcome.blade.php
```php
<x-app-layout>
    <h1 class="mt-4 text-4xl font-semibold text-center">{{ setting('site.title') }}</h1>
</x-app-layout>
```

Para cambiar el Logotipo esto se hace en resources/views/my-menu.blade.php
La primer forma de hacerlo es:
```php
 <a href="/" class="flex items-center">
    <img src="{{ Storage::url(setting('site.logo')) }}" class="h-6 mr-3 sm:h-10" alt="Flowbite Logo" />
</a>
```

La segunda es con el facade de Voyager, esto toma la configuración que este en config/voyager.php:
```php
<a href="/" class="flex items-center">
    <img src="{{ Voyager::image(setting('site.logo')) }}" class="h-6 mr-3 sm:h-10" alt="Flowbite Logo" />
</a> 
```

### Asignar mas parámetros a settings
Al final de la pagina de Settings tenemos la opción de agregar parámetros propios, por ejemplo vamos agregar:
Nombre: Titulo
Clave: title
Tipo: Caja de Texto
Grupo: Prueba


