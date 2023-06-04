---
title: "Add Property Database and Page Design Part 6"
date: 2023-05-26T08:53:10-07:00
draft: false
menuTitle: "58. Add Property Database and Page Design Part 6"
weight: 58
---

Relacionar la tabla de "property_types", "amenities", y de la tabla "users" vamos a relacionar al agente donde su 'role' sea un "agent" y que este "activo" (en campo 'status'), pasando los datos desde el controlador app/Http/Controllers/Backend/PropertyController.php
```php
// Añadir Propiedad
public function AddProperty(){

    $propertytype = PropertyType::latest()->get();
    $amenities = Amenities::latest()->get();
    $activeAgent = User::where('status','active')->where('role','agent')->latest()->get();

    return view('backend.property.add_property',compact('propertytype','amenities','activeAgent'));
} 
```
Para la seleccion de Tipo de Propiedad y Agente lo podemos resolver con una seleccion single:
en resources/views/backend/property/add_property.blade.php
```php
{{-- Row 6, Tipo de Propiedad, Comodidades, Agente --}}
<div class="row">

    {{-- Property Type --}}
    <div class="col-sm-4">
        <div class="mb-3">
            <label class="form-label">Tipo de Propiedad</label>
            <select name="ptype_id" class="form-select" id="exampleFormControlSelect1">
                <option selected="" disabled="">Seleccionar Tipo</option>
                @foreach($propertytype as $ptype)
                    <option value="{{ $ptype->id }}">{{ $ptype->type_name }}</option>
                @endforeach
            </select>
        </div>
    </div><!-- Col -->


    {{-- Agent --}}
    <div class="col-sm-4">
        <div class="mb-3">
            <label class="form-label">Agente</label>
            <select name="agent_id" class="form-select" id="exampleFormControlSelect1">
                <option selected="" disabled="">Seleccionar Agente</option>
                @foreach($activeAgent as $agent)
                    <option value="{{ $agent->id }}">{{ $agent->name }}</option>
                @endforeach
            </select>
        </div>
    </div><!-- Col -->

</div><!-- Row --> 
```
Para la seleccion de Comodidades lo podemos resolver con una seleccion multiple:
en resources/views/backend/property/add_property.blade.php
La estructura la podemos sacar de matreial de ejercicio en:
~/Sites/recursos/udemy/Laravel 10 - Build Real Estate Property Listing Project A-Z/Course+Excise+Files/Course Excise Files/Backend Theme/Main/pages/forms/advanced-elements.html 
usando "Multiple select using select 2"
Tenemos ue cargar el css y js especifico para este tipo de Multiple Select
```php
{{-- Property Amenities --}}
<div class="col-sm-4">
    <label class="form-label">Comodidades</label>
    <select name="amenities_id[]" class="js-example-basic-multiple form-select" multiple="multiple" data-width="100%">
        @foreach($amenities as $ameni)
            <option value="{{ $ameni->id }}">{{ $ameni->amenities_name }}</option>
        @endforeach
    </select>
</div><!-- Col -->
```
Y para soportar la seleccion multiple en resources/views/admin/admin_dashboard.blade.php:
```php
...
{{-- Plugins para la "Multiple select using select 2" que usamos en Property Amenities en resources/views/backend/property/add_property.blade.php --}}
<link rel="stylesheet" href="{{ asset('backend/assets/vendors/select2/select2.min.css') }}">
<link rel="stylesheet" href="{{ asset('backend/assets/vendors/jquery-tags-input/jquery.tagsinput.min.css') }}"> 
...
Y antes de que cierre el Body tag colocamos el codigo JS
{{-- Plugins para la "Multiple select using select 2" que usamos en Property Amenities en resources/views/backend/property/add_property.blade.php --}}
{{-- input tags --}}
<script src="{{ asset('backend/assets/vendors/inputmask/jquery.inputmask.min.js') }}"></script>
<script src="{{ asset('backend/assets/vendors/select2/select2.min.js') }}"></script>
<script src="{{ asset('backend/assets/vendors/typeahead.js/typeahead.bundle.min.js') }}"></script>
<script src="{{ asset('backend/assets/vendors/jquery-tags-input/jquery.tagsinput.min.js') }}"></script>
<script src="{{ asset('backend/assets/js/inputmask.js') }}"></script>
<script src="{{ asset('backend/assets/js/select2.js') }}"></script>
<script src="{{ asset('backend/assets/js/typeahead.js') }}"></script>
<script src="{{ asset('backend/assets/js/tags-input.js') }}"></script>
```

Listo!
