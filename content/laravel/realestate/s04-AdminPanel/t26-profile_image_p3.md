---
title: "Admin Profile & Image Update Part 3"
date: 2023-05-25T17:32:45-07:00
draft: false    
menuTitle: "26. Admin Profile & Image Update Part 3"
weight: 50
---

Vamos a trabajar en la forma de la derecha "UPDATE ADMIN PROFILE"
En resources/views/admin/admin_profile_view.blade.php:
```php
@extends('admin.admin_dashboard')
@section('admin')

<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.4/jquery.min.js"></script>
</head>

{{-- Contenido del profile form html --}}
<div class="page-content">

    <div class="row profile-body">

        <!-- left wrapper - Datos actuales del perfil -->
        <div class="d-none d-md-block col-md-4 col-xl-4 left-wrapper">
            <div class="card rounded">
                <div class="card-body">
                    <div class="d-flex align-items-center justify-content-between mb-2">

                        {{-- Foto --}}
                        <div>
                            <img class="wd-100 rounded-circle" src="{{ (!empty($profileData->photo)) ? url('upload/admin_images/'.$profileData->photo) : url('upload/no_image.jpg') }}" alt="profile">
                            <span class="h4 ms-3">{{ $profileData->username }}</span>
                        </div>

                    </div>

                    <div class="mt-3">
                        <label class="tx-11 fw-bolder mb-0 text-uppercase">Name:</label>
                        <p class="text-muted">{{ $profileData->name }}</p>
                    </div>
                    <div class="mt-3">
                        <label class="tx-11 fw-bolder mb-0 text-uppercase">Email:</label>
                        <p class="text-muted">{{ $profileData->email }}</p>
                    </div>
                    <div class="mt-3">
                        <label class="tx-11 fw-bolder mb-0 text-uppercase">Phone:</label>
                        <p class="text-muted">{{ $profileData->phone }}</p>
                    </div>
                    <div class="mt-3">
                        <label class="tx-11 fw-bolder mb-0 text-uppercase">Address:</label>
                        <p class="text-muted">{{ $profileData->address }}</p>
                    </div>
                    <div class="mt-3 d-flex social-links">
                        <a href="javascript:;" class="btn btn-icon border btn-xs me-2">
                            <i data-feather="github"></i>
                        </a>
                        <a href="javascript:;" class="btn btn-icon border btn-xs me-2">
                            <i data-feather="twitter"></i>
                        </a>
                        <a href="javascript:;" class="btn btn-icon border btn-xs me-2">
                            <i data-feather="instagram"></i>
                        </a>
                    </div>
                </div>
            </div>
        </div>

        <!-- wrapper derecha datos para editar -->
        <div class="col-md-8 col-xl-8 middle-wrapper">
            <div class="row">

                <div class="card">
                    <div class="card-body">

                        <h6 class="card-title">Update Admin Profile</h6>

                        <form class="forms-sample">

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

                    </div>
                </div>

            </div>
        </div>

    </div>

</div>

<script type="text/javascript">

    $(document).ready(function(){
        $('#image').change(function(e){
            var reader = new FileReader();
            reader.onload = function(e){
                $('#showImage').attr('src',e.target.result);
            }
            reader.readAsDataURL(e.target.files['0']);
        });
    });

</script>

@endsection
```
Usamos java script para poder actualizar la imagen de la photo, y su usamos JS tenemos que traernos
Jquery, el cual lo copiamos de w3 schools.

## 26. Admin Profile & Image Update Part 4
Agregar el mrtodo de post y la ruta a la forma en resources/views/admin/admin_profile_view.blade.php
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
