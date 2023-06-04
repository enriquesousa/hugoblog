---
title: "Install Image Intervention Package"
date: 2023-05-26T08:54:02-07:00
draft: false
menuTitle: "62. Install Image Intervention Package"
weight: 62
---

Utilizar el paquete de Laravel Intervention Packeage para reizes las imagenes que vamos a subir!
https://image.intervention.io/v2
Intervention Image is an open source PHP image handling and manipulation library. It provides an easier and expressive way to create, edit, and compose images and supports currently the two most common image processing libraries GD Library and Imagick.
Instalar el paquete:
```php
composer require intervention/image 
```
After you have installed Intervention Image, open your Laravel config file config/app.php and add the following lines.
In the $providers array add the service providers for this package.
En config/app.php, en Autoloaded Service Providers - Package Service Providers, agregar:
```php
/*
* Package Service Providers...
*/
Intervention\Image\ImageServiceProvider::class,
```
Add the facade of this package to the $aliases array, en la seccion de Class Aliases.
```php
'aliases' => Facade::defaultAliases()->merge([
    // 'Example' => App\Facades\Example::class,
    'Image' => Intervention\Image\Facades\Image::class
])->toArray(),
```
By default Intervention Image uses PHP's GD library extension to process all images. If you want to switch to Imagick, you can pull a configuration file into your application by running one of the following artisan command.
Publish configuration in Laravel
```php
php artisan vendor:publish --provider="Intervention\Image\ImageServiceProviderLaravelRecent"
```
Copying file [vendor/intervention/image/src/config/config.php] to [config/image.php] ........................................................ DONE

Listo!
