---
title: "Property Type Crud Part 1"
date: 2023-05-26T08:49:55-07:00
draft: false
menuTitle: "44. Property Type Crud Part 1"
weight: 44
---
Crear un nuevo controlador
```php
php artisan make:controller Backend/PropertyTypeController 
```
Crear un modelo y una tabla de migración
```php
php artisan make:model PropertyType -m 
```
En app/Models/PropertyType.php
```php
<?php
namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;

class PropertyType extends Model
{
    use HasFactory;
    // Para que todos los campos sean fillables
    protected $guarded = [];
}
```
En database/migrations/2023_05_31_212917_create_property_types_table.php
```php
Schema::create('property_types', function (Blueprint $table) {
    $table->id();

    $table->string('type_name');
    $table->string('type_icon')->nullable();

    $table->timestamps();
}); 
```
Hacer la migración:
```php
php artisan migrate 
```
En app/Http/Controllers/Backend/PropertyTypeController.php
```php
use Illuminate\Support\Facades\Auth;
use App\Models\PropertyType;

class PropertyTypeController extends Controller
{
    // Tomar todos los datos de la tabla property_types
    public function AllType(){

        $types = PropertyType::latest()->get();

        return view('backend.type.all_type', compact('types'));
    }

}
```
En resources/views/backend/type/all_type.blade.php
```php
@extends('admin.admin_dashboard')
@section('admin')



@endsection 
```

Añadir otro grupo de rutas solo para admins
En routes/web.php
```php
// Admin group middleware
Route::middleware(['auth','role:admin'])->group(function(){

    // Property All Routes
    Route::controller(PropertyTypeController::class)->group(function(){

        Route::get('/all/type', 'AllType')->name('all.type');

    });

}); 
```
Listo!

