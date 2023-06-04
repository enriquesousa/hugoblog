---
title: "Add Property Database Design Part 2"
date: 2023-05-26T08:52:25-07:00
draft: false
menuTitle: "53. Add Property Database Design Part 2"
weight: 53
---

Crear una nueva tabla para almacenar las multiples imágenes asociadas a una propiedad.
```php
php artisan make:model MultiImage -m 
```
En app/Models/MultiImage.php
```php
// Para que todos los campos sean fill-ables
protected $guarded = []; 
```
Y en la tabla database/migrations/2023_06_02_013822_create_multi_images_table.php
```php
public function up(): void
{
    Schema::create('multi_images', function (Blueprint $table) {
        $table->id();

        $table->integer('property_id');
        $table->string('photo_name');

        $table->timestamps();
    });
} 
```
Crear otra tabla para los lugares cercanos (neighborhood).
```php
php artisan make:model Facility -m
```
Los campos en esta tabla database/migrations/2023_06_02_014659_create_facilities_table.php
```php
public function up(): void
{
    Schema::create('facilities', function (Blueprint $table) {
        $table->id();

        $table->integer('property_id');
        $table->string('facility_name')->nullable();
        $table->string('distance')->nullable();

        $table->timestamps();
    });
} 
```

Finalmente hacer las migraciones:
```php
php artisan migrate 
```
Se crearon las tres tablas con éxito:
```php
2023_06_02_011542_create_properties_table ....................... 89ms DONE
2023_06_02_013822_create_multi_images_table ..................... 24ms DONE
2023_06_02_014659_create_facilities_table ....................... 30ms DONE 
```
Listo!

