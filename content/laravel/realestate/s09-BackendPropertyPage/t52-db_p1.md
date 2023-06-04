---
title: "Add Property Database Design Part 1"
date: 2023-05-26T08:52:12-07:00
draft: false
menuTitle: "52. Add Property Database Design Part 1"
weight: 52
---

Crear un controlador nuevo:
```php
php artisan make:controller Backend/PropertyController 
```
Crear una nueva tabla, esto se hace creando su modelo con migración:
```php
php artisan make:model Property -m 
```
En el modelo app/Models/Property.php:
```php
class Property extends Model
{
    use HasFactory;

    // Para que todos los campos sean fill-ables
    protected $guarded = [];

} 
```
Los campos que vamos a crear en la tabla database/migrations/2023_06_02_011542_create_properties_table.php
```php
public function up(): void
{
    Schema::create('properties', function (Blueprint $table) {
        $table->id();

        $table->string('ptype_id');
        $table->string('amenities_id');
        $table->string('property_name');
        $table->string('property_slug');
        $table->string('property_code');
        $table->string('property_status');
        $table->string('lowest_price')->nullable();
        $table->string('max_price')->nullable();
        $table->string('property_thambnail');
        $table->string('short_descp')->nullable();
        $table->text('long_descp')->nullable();
        $table->string('bedrooms')->nullable();
        $table->string('bathrooms')->nullable();
        $table->string('garage')->nullable();
        $table->string('garage_size')->nullable();
        $table->string('property_size')->nullable();
        $table->string('property_video')->nullable();
        $table->string('address')->nullable();
        $table->string('city')->nullable();
        $table->string('state')->nullable();
        $table->string('postal_code')->nullable();
        $table->string('neighborhood')->nullable();
        $table->string('latitude')->nullable();
        $table->string('longitude')->nullable();
        $table->string('featured')->nullable();
        $table->string('hot')->nullable();
        $table->integer('agent_id')->nullable();
        $table->string('status')->default(0);

        $table->timestamps();
    });
} 
```
Listo!

