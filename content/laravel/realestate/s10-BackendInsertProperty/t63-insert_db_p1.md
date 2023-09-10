---
title: "Property Insert Database Part 1"
date: 2023-05-26T08:54:15-07:00
draft: false
menuTitle: "63. Property Insert Database Part 1"
weight: 63
---

Empezando por crear la ruta en la forma de captura en resources/views/backend/property/add_property.blade.php
```php
...
<form method="POST" action="{{ route('store.property') }}" id="myForm" enctype="multipart/form-data">
@csrf
... 
```
Definimos la ruta en routes/web.php
```php
Route::post('/store/property', 'StoreProperty')->name('store.property'); 
```
Empezamos a crear el metodo StoreProperty() en app/Http/Controllers/Backend/PropertyController.php
```php
...
use Intervention\Image\Facades\Image;
...
// Store Property
public function StoreProperty(Request $request){
    $image = $request->file('property_thambnail');
    // crear un unique id para la imagen
    $name_gen = hexdec(uniqid()).'.'.$image->getClientOriginalExtension();
    Image::make($image)->resize(370,250)->save('upload/property/thambnail/'.$name_gen);
    $save_url = 'upload/property/thambnail/'.$name_gen;
} 
```
Listo!