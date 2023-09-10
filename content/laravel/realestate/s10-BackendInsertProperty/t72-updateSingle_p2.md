---
title: "Manage Property Update Single and Multiple Image Part 2"
date: 2023-05-26T08:55:31-07:00
draft: false
menuTitle: "72. Manage Property Update Single and Multiple Image Part 2"
weight: 72
---

Vamos a crear el metodo para actualizar solo la Imagen Miniatura
En resources/views/backend/property/edit_property.blade.php
```php
<form method="POST" action="{{ route('update.property.thumbnail') }}" id="myForm" enctype="multipart/form-data">
```
Creamos la ruta en routes/web.php
```php
Route::post('/update/property/thumbnail', 'UpdatePropertyThumbnail')->name('update.property.thumbnail'); 
```
Creamos el metodo en el controlador app/Http/Controllers/Backend/PropertyController.php
```php
// Update Property Thumbnail
public function UpdatePropertyThumbnail(Request $request){

    $pro_id = $request->id;
    $oldImage = $request->old_img;

    $image = $request->file('property_thambnail');
    $name_gen = hexdec(uniqid()).'.'.$image->getClientOriginalExtension(); // crear un unique id para la imagen
    Image::make($image)->resize(370,250)->save('upload/property/thambnail/'.$name_gen);
    $save_url = 'upload/property/thambnail/'.$name_gen;

    // Remover la imagen anterior
    if (file_exists($oldImage)) {
        unlink($oldImage);
    }

    Property::findOrFail($pro_id)->update([

        'property_thambnail' => $save_url,
        'updated_at' => Carbon::now(),

    ]);

    $notification = array(
        'message' => 'La imagen miniatura fue actualizada con éxito!',
        'alert-type' => 'success'
    );

    return redirect()->back()->with($notification);
} 
```
Listo!
