---
title: "Property Insert Database Part 3"
date: 2023-05-26T08:54:32-07:00
draft: false
menuTitle: "65. Property Insert Database Part 3"
weight: 65
---

Insertar las imágenes multiples en la tabla 'multi_images'
En app/Http/Controllers/Backend/PropertyController.php
```php
// Insertar datos a tabla 'multi_images', Multiple Image Upload From Here
$images = $request->file('multi_img');
foreach ($images as $img) {

    $make_name = hexdec(uniqid()) . '.' . $img->getClientOriginalExtension();
    Image::make($img)->resize(770, 520)->save('upload/property/multi-image/' . $make_name);
    $uploadPath = 'upload/property/multi-image/' . $make_name;

    MultiImage::insert([
        'property_id' => $property_id,
        'photo_name' => $uploadPath,
        'created_at' => Carbon::now(),
    ]);

} // End Foreach 
```
Listo!