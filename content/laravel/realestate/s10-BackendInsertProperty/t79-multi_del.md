---
title: "Property Delete With Multiple Image"
date: 2023-05-26T08:56:39-07:00
draft: false
menuTitle: "79. Property Delete With Multiple Image"
weight: 79
---

Eliminar una Propiedad.
Empezamos por determinar una ruta en resources/views/backend/property/all_property.blade.php
```php
<a href="{{ route('delete.property',$item->id) }}" class="btn btn-inverse-danger" id="delete">Borrar</a> 
```

Creamos su ruta en routes/web.php
```php
Route::get('/delete/property/{id}', 'DeleteProperty')->name('delete.property'); 
```

Creamos su controlador en app/Http/Controllers/Backend/PropertyController.php
```php
// Delete Property
public function DeleteProperty($id){

    // Encontrar el registro en la tabla 'properties' y eliminarlo con todo y foto de thumbnail
    $property = Property::findOrFail($id);
    unlink($property->property_thambnail);
    Property::findOrFail($id)->delete();

    // Ahora eliminar todas las multi fotos relacionadas con este registro $id
    $images = MultiImage::where('property_id',$id)->get();
    foreach ($images as $img) {
        unlink($img->photo_name);
        MultiImage::where('property_id',$id)->delete();
    }

    // Ahora eliminar todos los facilities de tabla 'facilities' donde 'property_id' sea igual al $id
    $comodidades = Facility::where('property_id',$id)->get();
    foreach ($comodidades as $item) {
        $item->facility_name;
        Facility::where('property_id',$id)->delete();
    }

    $notification = array(
        'message' => 'Propiedad Eliminada con éxito!',
        'alert-type' => 'success'
    );
    return redirect()->back()->with($notification);
} 
```
Listo!
