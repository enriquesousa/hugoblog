---
title: "Manage Property Update Single and Multiple Image Part 4"
date: 2023-05-26T08:55:55-07:00
draft: false
menuTitle: "74. Manage Property Update Single and Multiple Image Part 4"
weight: 74
---

Cambiar el formato del input file, al estilo de nuestra plantilla usando la clase class="form-control"
En resources/views/backend/property/edit_property.blade.php
```php
<td>
    <input type="file" class="form-control" name="multi_img[{{ $img->id }}]">
</td>
```
La ruta en routes/web.php
```php
Route::post('/update/property/multi-image', 'UpdatePropertyMultiImage')->name('update.property.multi-image'); 
```
Y el metodo en app/Http/Controllers/Backend/PropertyController.php
```php
// Update Property Multi Image
public function UpdatePropertyMultiImage(Request $request){

    $imgs = $request->multi_img;

    foreach ($imgs as $id => $img) {
        // get img que vamos a unlink (reemplazar)
        $imgDel = MultiImage::findOrFail($id);
        unlink($imgDel->photo_name);

        $make_name = hexdec(uniqid()) . '.' . $img->getClientOriginalExtension();
        Image::make($img)->resize(770, 520)->save('upload/property/multi-image/' . $make_name);
        $uploadPath = 'upload/property/multi-image/' . $make_name;

        MultiImage::where('id', $id)->update([
            'photo_name' => $uploadPath,
            'updated_at' => Carbon::now(),
        ]);
    } //End foreach

    $notification = array(
        'message' => 'La multi-imagen fue actualizada con éxito!',
        'alert-type' => 'success'
    );

    return redirect()->back()->with($notification);
} 
```
Listo!
