---
title: "Adding Toaster In For View Message"
date: 2023-05-25T17:33:01-07:00
draft: false    
menuTitle: "28. Adding Toaster In For View Message"
weight: 60
---
Agregar toaster cdn en resources/views/admin/admin_dashboard.blade.php:
```php
<head>
  	...
    <link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.css" >
</head>

<body>
	...
	
	{{-- Toaster cdn --}}
	    <script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>
	
	    {{-- Toaster script --}}
	    <script>
	        @if(Session::has('message'))
	            var type = "{{ Session::get('alert-type','info') }}"
	                switch(type){
	                case 'info':
	                toastr.info(" {{ Session::get('message') }} ");
	                break;
	
	                case 'success':
	                toastr.success(" {{ Session::get('message') }} ");
	                break;
	
	                case 'warning':
	                toastr.warning(" {{ Session::get('message') }} ");
	                break;
	
	                case 'error':
	                toastr.error(" {{ Session::get('message') }} ");
	                break;
	            }
	        @endif
	    </script>

</body>
```

Y en app/Http/Controllers/AdminController.php
```php
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
        unlink(public_path('upload/admin_images/'.$data->photo)); // para borrar la imagen anterior
        $filename = date('YmdHi').$file->getClientOriginalName();
        $file->move(public_path('upload/admin_images'), $filename);
        $data['photo'] = $filename; //Guardar a Base de Datos
    }

    $data->save();

    $notification = array(
        'message' => 'Perfil de Admin Actualizado Correctamente',
        'alert-type' => 'success'
    );

    return redirect()->back()->with($notification);
}
```

Para actualizar los datos del profileDropdown menu en resources/views/admin/body/header.blade.php
```php
...
@php

    $id = Auth::user()->id;
    $profileData = App\Models\User::find($id);

@endphp

{{-- profileDropdown --}}
<li class="nav-item dropdown">

    {{-- Imagen de Perfil de Arriba Header --}}
    <a class="nav-link dropdown-toggle" href="#" id="profileDropdown" role="button"
        data-bs-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
        <img class="wd-30 ht-30 rounded-circle" src="{{ (!empty($profileData->photo)) ? url('upload/admin_images/'.$profileData->photo) : url('upload/no_image.jpg') }}" alt="profile">
    </a>

    <div class="dropdown-menu p-0" aria-labelledby="profileDropdown">

        <div class="d-flex flex-column align-items-center border-bottom px-5 py-3">
            {{-- Imagen de Perfil de Abajo Menu --}}
            <div class="mb-3">
                <img class="wd-80 ht-80 rounded-circle" src="{{ (!empty($profileData->photo)) ? url('upload/admin_images/'.$profileData->photo) : url('upload/no_image.jpg') }}" alt="">
            </div>
            {{-- nombre y correo --}}
            <div class="text-center">
                <p class="tx-16 fw-bolder">{{ $profileData->name }}</p>
                <p class="tx-12 text-muted">{{ $profileData->email }}</p>
            </div>
        </div>

        {{-- menus --}}
        <ul class="list-unstyled p-1">

            {{-- Profile --}}
            <li class="dropdown-item py-2">
                <a href="{{ route('admin.profile') }}" class="text-body ms-0">
                    <i class="me-2 icon-md" data-feather="user"></i>
                    <span>Profile</span>
                </a>
            </li>

			...
			
        </ul>

    </div>
</li>
```
Listo!
