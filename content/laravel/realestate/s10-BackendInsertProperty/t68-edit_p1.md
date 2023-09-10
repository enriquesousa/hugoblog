---
title: "Manage Property Edit Option Part 1"
date: 2023-05-26T08:54:54-07:00
draft: false
menuTitle: "68. Manage Property Edit Option Part 1"
weight: 68
---

En resources/views/backend/property/all_property.blade.php
```php
<a href="{{ route('edit.property',$item->id) }}" class="btn btn-inverse-warning">Editar</a> 
```

En routes/web.php
```php
Route::get('/edit/property/{id}', 'EditProperty')->name('edit.property'); 
``` 

En app/Http/Controllers/Backend/PropertyController.php
```php
// Editar Datos de la Propiedad
public function EditProperty($id){

    $property = Property::findOrFail($id);
    $propertytype = PropertyType::latest()->get();
    $amenities = Amenities::latest()->get();
    $activeAgent = User::where('status','active')->where('role','agent')->latest()->get();

    return view('backend.property.edit_property',compact('property','propertytype','amenities','activeAgent'));

} 
``` 

Copiar el código add_property y hacer algunas modificaciones en resources/views/backend/property/edit_property.blade.php, desplegar los campos para poder ser editados, agregando los values a cada input:
```php
value="{{ $property->property_name }}"
value="{{ $property->lowest_price }}"
value="{{ $property->max_price }}"
Etc..
```

Para desplegar html en textarea de long_descp tenemos que usar la sintaxis
```php
{!! $str!!} 
```
Quedaria asi:
```php
{{-- Long Description --}}
<div class="col-sm-12">
    <div class="mb-3">
        <label class="form-label text-warning">Descripción Larga</label>
        <textarea name="long_descp" class="form-control" id="tinymceExample" rows="10">
            {!! $property->long_descp !!}
        </textarea>
    </div>
</div><!-- Col --> 
```
Listo!

