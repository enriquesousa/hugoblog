---
title: "Manage Property Update Single and Multiple Image Part 3"
date: 2023-05-26T08:55:47-07:00
draft: false
menuTitle: "73. Manage Property Update Single and Multiple Image Part 3"
weight: 73
---

Ahora vamos a update las imágenes multiples de la propiedad
Preparar las imágenes que vamos mandar a la vista en el método EditProperty en app/Http/Controllers/Backend/PropertyController.php
```php
public function EditProperty($id){
    ...
    // Cargar las imágenes de la tabla 'multi_images' que correspondan con el $id de la propiedad editada
    $multiImage = MultiImage::where('property_id',$id)->get();
    ...
    return view('backend.property.edit_property',compact('property','propertytype','amenities','activeAgent', 'property_ami', 'multiImage'));
} 
```
Y en la vista crear una forma mas, resources/views/backend/property/edit_property.blade.php
```php
<form method="POST" action="{{ route('update.property.thumbnail') }}" id="myForm" enctype="multipart/form-data">
    @csrf

    <table class="table table-striped">
        <thead>
            <tr>
                <th>Serie</th>
                <th>Imagen</th>
                <th>Cambiar Imagen</th>
                <th>Actualizar - Eliminar</th>
            </tr>
        </thead>
        <tbody>
            @foreach ($multiImage as $key => $img)
                <tr>
                    <td>{{ $key+1 }}</td>
                    <td class="py-1">
                        <img src="{{ asset($img->photo_name) }}" alt="image" style="width: 50px; height: 50px;">
                    </td>
                    <td>
                        <input type="file" class="form-group" name="multi_img">
                    </td>
                    <td>
                        <input type="submit" class="btn btn-primary px-4" value="Actualizar Imagen">
                        <a href="" class="btn btn-danger" id="delete">Eliminar</a>
                    </td>
                </tr>
            @endforeach
        </tbody>
    </table>

    <br><br>
    <button type="submit" class="btn btn-primary">Salvar Cambios</button>

</form> 
```

En la siguiente lección crearemos las rutas.
Listo!
    

