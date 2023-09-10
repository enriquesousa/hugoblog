---
title: "Manage Property Edit Option Part 2"
date: 2023-05-26T08:55:05-07:00
draft: false
menuTitle: "69. Manage Property Edit Option Part 2"
weight: 69
---

Para colocar la selección correcta para Property Status, ya sea **rent** or **buy**
En resources/views/backend/property/edit_property.blade.php
```php
{{-- Property Status --}}
<div class="col-sm-6">
    <div class="form-group mb-3">
        <label class="form-label text-warning">Estatus</label>
        <select name="property_status" class="form-select"
            id="exampleFormControlSelect1">
            <option selected="" disabled="">Seleccionar Estatus</option>
            <option value="rent" {{ $property->property_status == 'rent' ? 'selected' : '' }}>Para Renta</option>
            <option value="buy" {{ $property->property_status == 'buy' ? 'selected' : '' }}>Para Compra</option>
        </select>
    </div>
</div><!-- Col --> 
```

Para el Tipo de Propiedad podemos hacer:
```php
{{-- Property Type --}}
<div class="col-sm-4">
    <div class="form-group mb-3">
        <label class="form-label text-warning">Tipo de Propiedad</label>
        <select name="ptype_id" class="form-select" id="exampleFormControlSelect1">
            <option selected="" disabled="">Seleccionar Tipo</option>
            @foreach($propertytype as $ptype)
                <option value="{{ $ptype->id }}" {{ $ptype->id == $property->ptype_id ? 'selected' : '' }}>{{ $ptype->type_name }}</option>
            @endforeach
        </select>
    </div>
</div><!-- Col -->
```

Para el Agente igual
```php
{{-- Agent --}}
<div class="col-sm-4">
    <div class="mb-3">
        <label class="form-label text-warning">Agente</label>
        <select name="agent_id" class="form-select" id="exampleFormControlSelect1">
            <option selected="" disabled="">Seleccionar Agente</option>
            @foreach($activeAgent as $agent)
                <option value="{{ $agent->id }}" {{ $agent->id == $property->agent_id ? 'selected' : '' }}>{{ $agent->name }}</option>
            @endforeach
        </select>
    </div>
</div><!-- Col --> 
```

Para las coomodidades, ahora hay que expander en app/Http/Controllers/Backend/PropertyController.php
```php
...
$type = $property->amenities_id;
$property_ami = explode(',', $type);
..
Y pasarla en la funcion compact
return view('backend.property.edit_property',compact('property','propertytype','amenities','activeAgent', 'property_ami'));
```
Y en resources/views/backend/property/edit_property.blade.php
```php
{{-- Property Amenities --}}
<div class="col-sm-4">
    <label class="form-label text-warning">Comodidades</label>
    <select name="amenities_id[]" class="js-example-basic-multiple form-select" multiple="multiple" data-width="100%">
        @foreach($amenities as $ameni)
            <option value="{{ $ameni->id }}" {{ (in_array($ameni->id, $property_ami)) ? 'selected' : '' }}>{{ $ameni->amenities_name }}</option>
        @endforeach
    </select>
</div><!-- Col --> 
```

Ahora los check de Features Property y Hot Property
```php
{{-- Checkboxes Features Property y Hot Property --}}
<div class="mb-3">

    <div class="form-check form-check-inline">
        <input type="checkbox" name="featured" value="1" class="form-check-input" id="checkInline1" {{ $property->featured == 1 ? 'checked' : '' }}>
        <label class="form-check-label text-warning" for="checkInline1">
            Features Property
        </label>
    </div>

    <div class="form-check form-check-inline">
        <input type="checkbox" name="hot" value="1" class="form-check-input" id="checkInline" {{ $property->hot == 1 ? 'checked' : '' }}>
        <label class="form-check-label text-warning" for="checkInline">
            Hot Property
        </label>
    </div>

</div> 
```
Listo!
    