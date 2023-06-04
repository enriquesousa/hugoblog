---
title: "Admin Profile Change Password Part 1"
date: 2023-05-25T17:33:10-07:00
draft: false    
menuTitle: "29. Admin Profile Change Password Part 1"
weight: 65
---
resources/views/admin/admin_change_password.blade.php
```php
<form method="POST" action="{{ route('admin.profile.store') }}" class="forms-sample" enctype="multipart/form-data">
@csrf


    {{-- Old Password --}}
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Contraseña Vieja</label>
        <input type="password" name="old_password" class="form-control @error('old_password') is-invalid @enderror" id="old_password" autocomplete="off">
        @error('old_password')
            <span class="text-danger">{{ $message }}</span>
        @enderror
    </div>

    {{-- New Password --}}
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Contraseña Nueva</label>
        <input type="password" name="new_password" class="form-control @error('new_password') is-invalid @enderror" id="new_password" autocomplete="off">
        @error('new_password')
            <span class="text-danger">{{ $message }}</span>
        @enderror
    </div>

    {{-- Confirm Password --}}
    <div class="mb-3">
        <label for="exampleInputEmail1" class="form-label">Confirmar Contraseña Nueva</label>
        <input type="password" name="new_password_confirmation" class="form-control" id="new_password_confirmation" autocomplete="off">
    </div>

    <button type="submit" class="btn btn-primary me-2">Save Changes</button>

</form>
```
Listo!

