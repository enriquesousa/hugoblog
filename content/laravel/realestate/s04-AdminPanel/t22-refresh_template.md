---
title: "Refresh Admin Template"
date: 2023-05-25T17:32:14-07:00
draft: false    
menuTitle: "22. Refresh Admin Template"
weight: 30
---

Dejar en el sidebar unos cuantos menus, en resources/views/admin/body/sidebar.blade.php:
```php
<li class="nav-item nav-category">Main</li>
<li class="nav-item">
    <a href="{{ route('admin.dashboard') }}" class="nav-link">
        <i class="link-icon" data-feather="box"></i>
        <span class="link-title">Dashboard</span>
    </a>
</li>

{{-- * WEB APPS --}}

{{-- * COMPONENTS --}}

{{-- * DOCS --}}

```

Quitar algunas cosas del header resources/views/admin/body/header.blade.php:
```php
Quitar lo de los lenguajes
```

Dejar soloen resources/views/admin/index.blade.php:
```php
{{-- Row 2 --}}
<div class="row">
  {{-- Monthly sales --}} a  12 columnas

{{-- Row 3 --}}
<div class="row">
    {{-- Inbox --}}
  	{{-- Projects --}}
```