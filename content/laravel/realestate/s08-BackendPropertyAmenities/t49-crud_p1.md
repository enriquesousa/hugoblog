---
title: "Amenities Crud Part 1"
date: 2023-05-26T08:51:13-07:00
draft: false
menuTitle: "49. Amenities Crud Part 1"
weight: 49
---

Crear una nueva tabla y su modelo
```php
php artisan make:model Amenities -m 
```
Agregar un solo campo, en database/migrations/2023_06_01_151806_create_amenities_table.php
```php
$table->string('amenities_name'); 
```
En en modelo hacer fillable todos los campos, app/Models/Amenities.php
```php
protected $guarded = []; 
```
Hacer la migracion:
```php
php artisan migrate 
```
Varios archivos mas ver este commit en github.
Listo!
