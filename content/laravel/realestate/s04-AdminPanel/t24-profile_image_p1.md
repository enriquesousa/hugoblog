---
title: "Admin Profile & Image Update Part 1"
date: 2023-05-25T17:32:31-07:00
draft: false    
menuTitle: "24. Admin Profile & Image Update Part 1"
weight: 40
---

Agregar la ruta a resources/views/admin/body/header.blade.php:
```php
<li class="dropdown-item py-2">
    <a href="{{ route('admin.profile') }}" class="text-body ms-0">
        <i class="me-2 icon-md" data-feather="user"></i>
        <span>Profile</span>
    </a>
</li>
```
En routes/web.php:
```php
Route::get('/admin/profile', [AdminController::class, 'AdminProfile'])->name('admin.profile');
```
En app/Http/Controllers/AdminController.php:
```php
// Admin Profile
public function AdminProfile(){
    $id = Auth::user()->id;
    $profileData = User::find($id);
    return view('admin.admin_profile_view', compact('profileData'));
}
```
En resources/views/admin/admin_profile_view.blade.php:
```php
@extends('admin.admin_dashboard')
@section('admin')

@endsection
```
Si hacemos cambios de ruta en web.php y queremos checar los cambios, laravel nos
manda un error como este:
```Route [admin.profile] not defined.```
Para corregir esto correr:
```php artisan optimize```
Para limpiar el cache!
Listo!

