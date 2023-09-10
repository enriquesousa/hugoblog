---
title: "Manage Property Update Option"
date: 2023-05-26T08:55:14-07:00
draft: false
menuTitle: "70. Manage Property Update Option"
weight: 70
---

Actualizar la acción de la forma para poder redirigir-lo a una ruta de update!
En resources/views/backend/property/edit_property.blade.php
```php
<form method="POST" action="{{ route('update.property') }}" id="myForm" enctype="multipart/form-data">
@csrf
    
    {{-- Para capturar el id del record que queremos editar --}}
    <input type="hidden" name="id" value="{{ $property->id }}"> 

    ...

</form>
```

En routes/web.php
```php
Route::post('/update/property', 'UpdateProperty')->name('update.property'); 
```

Y el metodo Update en app/Http/Controllers/Backend/PropertyController.php
```php
// Update Property
public function UpdateProperty(Request $request){

    $amen = $request->amenities_id;
    $comodidades_str = implode(",", $amen);

    $property_id = $request->id;

    Property::findOrFail($property_id)->update([

        'ptype_id' => $request->ptype_id,
        'amenities_id' => $comodidades_str,
        'property_name' => $request->property_name,
        'property_slug' => strtolower(str_replace(' ', '-', $request->property_name)),
        'property_status' => $request->property_status,

        'lowest_price' => $request->lowest_price,
        'max_price' => $request->max_price,
        'short_descp' => $request->short_descp,
        'long_descp' => $request->long_descp,
        'bedrooms' => $request->bedrooms,
        'bathrooms' => $request->bathrooms,
        'garage' => $request->garage,
        'garage_size' => $request->garage_size,

        'property_size' => $request->property_size,
        'property_video' => $request->property_video,
        'address' => $request->address,
        'city' => $request->city,
        'state' => $request->state,
        'postal_code' => $request->postal_code,

        'neighborhood' => $request->neighborhood,
        'latitude' => $request->latitude,
        'longitude' => $request->longitude,
        'featured' => $request->featured,
        'hot' => $request->hot,
        'agent_id' => $request->agent_id,
        'updated_at' => Carbon::now(),

    ]);

    $notification = array(
        'message' => 'La propiedad se actualizo con éxito!',
        'alert-type' => 'success'
    );

    return redirect()->route('all.property')->with($notification);

} 
```
Listo!
