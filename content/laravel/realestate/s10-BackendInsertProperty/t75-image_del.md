---
title: "Multiple Image Delete"
date: 2023-05-26T08:56:04-07:00
draft: false
menuTitle: "75. Multiple Image Delete"
weight: 75
---

Eliminar una imagen de Multi Imagen
Primero asignar la ruta en el botón de borrar en resources/views/backend/property/edit_property.blade.php
```php
<td>
    <input type="submit" class="btn btn-primary px-4" value="Actualizar Imagen">
    <a href="{{ route('delete.property.multi-image', $img->id) }}" class="btn btn-danger" id="delete">Borrar</a>
</td> 
```
Definir la ruta en routes/web.php
```php
Route::get('/delete/property/multi-image/{id}', 'DeleteMultiImageProperty')->name('delete.property.multi-image'); 
```
Crear método en app/Http/Controllers/Backend/PropertyController.php
```php
// Eliminar una imagen de las de Multi Image de una propiedad
public function DeleteMultiImageProperty($id){
    $oldImg = MultiImage::findOrFail($id);
    unlink($oldImg->photo_name);

    MultiImage::findOrFail($id)->delete();

    $notification = array(
        'message' => 'La multi-imagen fue eliminada con éxito!',
        'alert-type' => 'success'
    );
    return redirect()->back()->with($notification);
} 
```
Listo!
