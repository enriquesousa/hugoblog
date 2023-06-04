---
title: "Add Property Database and Page Design Part 1"
date: 2023-05-26T08:52:34-07:00
draft: false
menuTitle: "54. Add Property Database and Page Design Part 1"
weight: 54
---

Añadir otra opción de menu en el sidebar 
en resources/views/admin/body/sidebar.blade.php
```php
{{-- Property --}}
<a class="nav-link" data-bs-toggle="collapse" href="#property" role="button" aria-expanded="false"
    aria-controls="emails">
    <i class="link-icon" data-feather="mail"></i>
    <span class="link-title">Propiedades</span>
    <i class="link-arrow" data-feather="chevron-down"></i>
</a>
<div class="collapse" id="property">
    <ul class="nav sub-menu">
        <li class="nav-item">
            <a href="{{ route('all.property') }}" class="nav-link">Todas las Propiedades</a>
        </li>
        <li class="nav-item">
            <a href="{{ route('add.amenities') }}" class="nav-link">Añadir una Comodidad</a>
        </li>
    </ul>
</div> 
```
Crear su ruta en web.php
```php
// Admin group middleware
Route::middleware(['auth','role:admin'])->group(function(){
    ...
    // Property All Routes
    Route::controller(PropertyController::class)->group(function(){
        Route::get('/all/property', 'AllProperty')->name('all.property');
    });

}); 
```
En el controlador app/Http/Controllers/Backend/PropertyController.php
```php
// All Property / Todas las Propiedades
public function AllProperty(){

    // Recuperar todos los datos de la tabla properties
    $property = Property::latest()->get();
    return view('backend.property.all_property', compact('property'));
} 
```
Crear la vista donde presentaremos los datos en una tabla resources/views/backend/property/all_property.blade.php
Listo!
