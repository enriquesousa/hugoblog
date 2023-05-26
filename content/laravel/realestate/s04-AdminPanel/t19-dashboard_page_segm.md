---
title: "Dashboard Page Segmentation"
date: 2023-05-25T17:31:49-07:00
draft: false
menuTitle: "19. Dashboard Page Segmentation"
weight: 15
---

Secmentar las secciones, creando:

- resources/views/admin/body/header.blade.php
- resources/views/admin/body/sidebar.blade.php
- resources/views/admin/body/footer.blade.php

En resources/views/admin/admin_dashboard.blade.php
```php
<div class="main-wrapper">

  <!-- partial:partials/_sidebar.html -->
  @include('admin.body.sidebar')

  <div class="page-wrapper">

    <!-- Header Top Bar partial:partials/_navbar.html -->
    @include('admin.body.header')

    {{-- Contenido --}}
    @yield('admin')

    <!-- partial:partials/_footer.html -->
    @include('admin.body.footer')

  </div>

</div>
```