---
title: "Property Insert Database Part 4"
date: 2023-05-26T08:54:39-07:00
draft: false
menuTitle: "66. Property Insert Database Part 4"
weight: 66
---

Agregar código en método para agregar las "Instalaciones Cercanas" 
En app/Http/Controllers/Backend/PropertyController.php
```php
use App\Models\Facility;


// Insertar datos a tabla 'facilities', Instalaciones cercanas - Facilities Add From Here
$facilities = Count($request->facility_name);
if ($facilities != NULL) {
    for ($i=0; $i < $facilities; $i++) {
        $fcount = new Facility();
        $fcount->property_id = $property_id;
        $fcount->facility_name = $request->facility_name[$i];
        $fcount->distance = $request->distance[$i];
        $fcount->save();
    }
}

$notification = array(
    'message' => 'La Propiedad fue añadida con éxito!',
    'alert-type' => 'success'
);

return back()->roue('all.property')->with($notification); 

```
Aquí usamos otro método para grabar los datos, de cualquier for de todas maneras se 
necesita soportar el objeto de Facility con:
```php
use App\Models\Facility; 
```
Listo!