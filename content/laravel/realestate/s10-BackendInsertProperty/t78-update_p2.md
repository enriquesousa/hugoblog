---
title: "Manage Property Update Facility Part 2"
date: 2023-05-26T08:56:32-07:00
draft: false
menuTitle: "78. Manage Property Update Facility Part 2"
weight: 78
---

Empezamos creando la ruta en la forma en resources/views/backend/property/edit_property.blade.php
```php
<form method="POST" action="{{ route('update.property.facilities') }}" id="myForm" enctype="multipart/form-data"> 
```

Añadir la ruta en routes/web.php
```php
Route::post('/store/property/facilities', 'UpdatePropertyFacilities')->name('update.property.facilities'); 
```

Agregar el método en app/Http/Controllers/Backend/PropertyController.php
```php
// Update Property Facilities
public function UpdatePropertyFacilities(Request $request){

    $pid = $request->id;

    if ($request->facility_name == NULL) {
        return redirect()->back();
    }else{
        // Borrar registro
        Facility::where('property_id', $pid)->delete();

        // Si queda algún registro Insertar datos a tabla 'facilities'
        $facilities = Count($request->facility_name);
        for ($i=0; $i < $facilities; $i++) {
            $fcount = new Facility();
            $fcount->property_id = $pid;
            $fcount->facility_name = $request->facility_name[$i];
            $fcount->distance = $request->distance[$i];
            $fcount->save();
        }
    }

    $notification = array(
        'message' => 'Instalaciones cercanas actualizadas con éxito!',
        'alert-type' => 'success'
    );
    return redirect()->back()->with($notification);
} 
```

El código de la forma en resources/views/backend/property/edit_property.blade.php
```php
<form method="POST" action="{{ route('update.property.facilities') }}" id="myForm" enctype="multipart/form-data">
@csrf

    {{-- Para capturar el id del record que queremos editar --}}
    <input type="hidden" name="id" value="{{ $property->id }}">

    @foreach ($facilities as $item)

    {{-- Facilities Option / Instalaciones Cercanas --}}
    <div class="row add_item">
        <div class="whole_extra_item_add" id="whole_extra_item_add">
            <div class="whole_extra_item_delete" id="whole_extra_item_delete">
                <div class="container mt-2">
                    <div class="row">
                        {{-- Input Instalación Cercana con un Select --}}
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
                        {{-- Input Distancia --}}
                        <div class="form-group col-md-4">
                            <label for="distance">Distancia</label>
                            <input type="text" name="distance[]" id="distance" class="form-control" value="{{ $item->distance }}">
                        </div>
                        {{-- Botones de Agregar y Remover --}}
                        <div class="form-group col-md-4" style="padding-top: 20px">
                            <span class="btn btn-success btn-sm addeventmore"><i class="fa fa-plus-circle">Agregar</i></span>
                            <span class="btn btn-danger btn-sm removeeventmore"><i class="fa fa-minus-circle">Remover</i></span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    @endforeach

    <br><br>
    <button type="submit" class="btn btn-primary">Salvar Cambios</button>

</form> 
```
Listo!
