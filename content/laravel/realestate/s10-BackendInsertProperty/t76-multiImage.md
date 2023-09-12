---
title: "Add Multi Image In Property"
date: 2023-05-26T08:56:15-07:00
draft: false
menuTitle: "76. Add Multi Image In Property"
weight: 76
---

Crear una forma mas para poder añadir un Multi-Image mas.
En resources/views/backend/property/edit_property.blade.php
```php
<form method="POST" action="{{ route('store.new.multi-image') }}" id="myForm" enctype="multipart/form-data">
@csrf

    <input type="hidden" name="imageId" value="{{ $property->id }}">

    <table class="table table-striped">
        <tbody>
            <tr>
                <td>
                    <input type="file" class="form-control" name="multi_img">
                </td>
                <td>
                    <input type="submit" class="btn btn-info px-4" value="Añadir Imagen">
                </td>
            </tr>
        </tbody>
    </table>

</form> 
```

Crear su ruta en routes/web.php
```php
Route::post('/store/new/multi-image', 'StoreNewMultiImage')->name('store.new.multi-image'); 
```

Crear el método en app/Http/Controllers/Backend/PropertyController.php
```php
// Store New Multi Image
public function StoreNewMultiImage(Request $request){

    $new_multi = $request->imageId;
    $image = $request->file('multi_img');


    if (!empty($image)) {

        $make_name = hexdec(uniqid()) . '.' . $image->getClientOriginalExtension();
        Image::make($image)->resize(770, 520)->save('upload/property/multi-image/' . $make_name);
        $uploadPath = 'upload/property/multi-image/' . $make_name;

        MultiImage::insert([
            'property_id' => $new_multi,
            'photo_name' => $uploadPath,
            'created_at' => Carbon::now(),
        ]);

        $notification = array(
            'message' => 'Multi-Imagen fue añadida con éxito!',
            'alert-type' => 'success'
        );

    } else {

        $notification = array(
            'message' => 'No hay Imagen para Añadir!',
            'alert-type' => 'warning'
        );
    }

    return redirect()->back()->with($notification);
}
```
Listo!

