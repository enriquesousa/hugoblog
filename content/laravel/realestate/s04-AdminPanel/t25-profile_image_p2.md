---
title: "Admin Profile & Image Update Part 2"
date: 2023-05-25T17:32:38-07:00
draft: false    
menuTitle: "25. Admin Profile & Image Update Part 2"
weight: 45
---

Customizar la forma izquierda de desplegar datos de perfil.
En resources/views/admin/admin_profile_view.blade.php:
```php
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
```

