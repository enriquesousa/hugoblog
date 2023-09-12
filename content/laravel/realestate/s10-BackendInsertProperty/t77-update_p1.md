---
title: "Manage Property Update Facility Part 1"
date: 2023-05-26T08:56:22-07:00
draft: false
menuTitle: "77. Manage Property Update Facility Part 1"
weight: 77
---

Update Facilities - Actualizar Instalaciones Cercanas
Agregar una nueva forma (row) al final de la pagina
En resources/views/backend/property/edit_property.blade.php
```php
{{-- Update Facilities - Actualizar Instalaciones Cercanas  --}}
<div class="page-content" style="margin-top: -35px">
    <div class="row profile-body">
        <!-- wrapper datos para editar con el total del ancho 12 columnas -->
        <div class="col-md-12 col-xl-12 middle-wrapper">
            <div class="row">
                <div class="card">
                    <div class="card-body">
                        <h6 class="card-title">Editar Instalaciones Cercanas de la Propiedad</h6>

                        <form method="POST" action="{{ route('update.property.thumbnail') }}" id="myForm" enctype="multipart/form-data">
                        @csrf

                        @foreach ($facilities as $item)

                            {{-- Facilities Option / Instalaciones Cercanas --}}
                            <div class="whole_extra_item_delete" id="whole_extra_item_delete">
                                <div class="container mt-2">
                                    <div class="row">

                                        <div class="form-group col-md-4">
                                            <label for="facility_name">Instalación Cercana</label>
                                            <select name="facility_name[]" id="facility_name" class="form-control">

                                                <option value="">Selecciona Instalación</option>
                                                <option value="Hospital" {{ $item->facility_name == 'Hospital' ? 'selected' : '' }}>Hospital</option>
                                                <option value="Super Mercado" {{ $item->facility_name == 'Super Mercado' ? 'selected' : '' }}>Super Mercado</option>
                                                <option value="Escuela" {{ $item->facility_name == 'Escuela' ? 'selected' : '' }}>Escuela</option>
                                                <option value="Entretenimiento" {{ $item->facility_name == 'Entretenimiento' ? 'selected' : '' }}>Entretenimiento</option>
                                                <option value="Farmacia" {{ $item->facility_name == 'Farmacia' ? 'selected' : '' }}>Farmacia</option>
                                                <option value="Aeropuerto" {{ $item->facility_name == 'Aeropuerto' ? 'selected' : '' }}>Aeropuerto</option>
                                                <option value="Estación de Tren" {{ $item->facility_name == 'Estación de Tren' ? 'selected' : '' }}>Estación de Tren</option>
                                                <option value="Parada de autobus" {{ $item->facility_name == 'Parada de autobus' ? 'selected' : '' }}>Parada de autobus</option>
                                                <option value="Playa" {{ $item->facility_name == 'Playa' ? 'selected' : '' }}>Playa</option>
                                                <option value="Centro Comercial" {{ $item->facility_name == 'Centro Comercial' ? 'selected' : '' }}>Centro Comercial</option>
                                                <option value="Banco" {{ $item->facility_name == 'Banco' ? 'selected' : '' }}>Banco</option>
                                                <option value="Cine" {{ $item->facility_name == 'Cine' ? 'selected' : '' }}>Cine</option>
                                                <option value="Restaurante" {{ $item->facility_name == 'Restaurante' ? 'selected' : '' }}>Restaurante</option>

                                            </select>
                                        </div>
                                        <div class="form-group col-md-4">
                                            <label for="distance">Distancia</label>
                                            <input type="text" name="distance[]" id="distance" class="form-control" value="{{ $item->distance }}">
                                        </div>
                                        <div class="form-group col-md-4" style="padding-top: 20px">
                                            <span class="btn btn-success btn-sm addeventmore"><i class="fa fa-plus-circle">Agregar</i></span>
                                            <span class="btn btn-danger btn-sm removeeventmore"><i class="fa fa-minus-circle">Remover</i></span>
                                        </div>
                                    </div>
                                </div>
                            </div>

                        @endforeach

                        </form>

                    </div>
                </div>
            </div>
        </div>
    </div>
</div>
```

Y en el método Edit pasar los facilities.
En app/Http/Controllers/Backend/PropertyController.php
```php
// Editar Datos de la Propiedad
public function EditProperty($id){

    // Cargar solo los datos de la tabla 'facilities' donde el 'property_id' es igual al $id de la Propiedad
    $facilities = Facility::where('property_id',$id)->get();

    // Cargar todos los datos de la tabla 'properties' donde el id es igual al $id pasado por la función
    $property = Property::findOrFail($id);

    $type = $property->amenities_id;
    $property_ami = explode(',', $type);

    // Cargar las imágenes de la tabla 'multi_images' que correspondan con el $id de la propiedad editada
    $multiImage = MultiImage::where('property_id',$id)->get();

    $propertytype = PropertyType::latest()->get();
    $amenities = Amenities::latest()->get();
    $activeAgent = User::where('status','active')->where('role','agent')->latest()->get();

    return view('backend.property.edit_property',compact('property','propertytype','amenities','activeAgent', 'property_ami', 'multiImage', 'facilities'));

} 
```
Listo!
