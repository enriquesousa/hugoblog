---
title: "Admin Profile & Image Update Part 4"
date: 2023-05-25T17:32:53-07:00
draft: false    
menuTitle: "27. Admin Profile & Image Update Part 4"
weight: 55
---

## Agregar el mrtodo de post y la ruta a la forma en resources/views/admin/admin_profile_view.blade.php

```php
<form method="POST" action="{{ route('admin.profile.store') }}" class="forms-sample" enctype="multipart/form-data">
@csrf

    {{-- username --}}
    <div class="mb-3">
        <label for="exampleInputUsername1" class="form-label">Username</label>
        <input type="text" name="username" class="form-control" id="exampleInputUsername1" autocomplete="off" value="{{ $profileData->username }}">
    </div>

    {{-- Name --}}
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Name</label>
        <input type="text" name="name" class="form-control" id="exampleInputUsername1" autocomplete="off" value="{{ $profileData->name }}">
    </div>

    {{-- Email --}}
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Email</label>
        <input type="email" name="email" class="form-control" id="exampleInputUsername1" autocomplete="off" value="{{ $profileData->email }}">
    </div>

    {{-- Phone --}}
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Phone</label>
        <input type="text" name="phone" class="form-control" id="exampleInputUsername1" autocomplete="off" value="{{ $profileData->phone }}">
    </div>

    {{-- Address --}}
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Address</label>
        <input type="text" name="address" class="form-control" id="exampleInputUsername1" autocomplete="off" value="{{ $profileData->address }}">
    </div>

    {{-- Seleccionar Photo --}}
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Photo</label>
        <input type="file" class="form-control" name="photo" id="image">
    </div>

    {{-- Desplegar Photo --}}
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label"></label>
        <img id="showImage" class="wd-80 rounded-circle" src="{{ (!empty($profileData->photo)) ? url('upload/admin_images/'.$profileData->photo) : url('upload/no_image.jpg') }}" alt="profile">
    </div>

    <button type="submit" class="btn btn-primary me-2">Save Changes</button>

</form>
```

Crear la ruta en routes/web.php
```php
Route::post('/admin/profile/store', [AdminController::class, 'AdminProfileStore'])->name('admin.profile.store');
```

Crear el metodo "AdminProfileStore" en app/Http/Controllers/AdminController.php
```php
// Admin Profile Store
public function AdminProfileStore(Request $request){
    $id = Auth::user()->id;
    $data = User::find($id);

    $data->username = $request->username;
    $data->name = $request->name;
    $data->email = $request->email;
    $data->phone = $request->phone;
    $data->address = $request->address;

    if ($request->file('photo')) {
        $file = $request->file('photo');
        $filename = date('YmdHi').$file->getClientOriginalName();
        $file->move(public_path('upload/admin_images'), $filename);
        $data['photo'] = $filename; //Guardar a Base de Datos
    }

    $data->save();

    return redirect()->back();
}
```
Listo!
