---
title: "Add Property Database and Page Design Part 7"
date: 2023-05-26T08:53:17-07:00
draft: false
menuTitle: "59. Add Property Database and Page Design Part 7"
weight: 59
---

Buscar una text area input para nuestros campos descripcion corta y larga.
Los drivers para tener un editor en el text area.
Para el txt area del long description:
~/Sites/recursos/udemy/Laravel 10 - Build Real Estate Property Listing Project A-Z/Course+Excise+Files/Course Excise Files/Backend Theme/Main/pages/forms/editors.html
En resources/views/backend/property/add_property.blade.php
```php
{{-- Short Description --}}
<div class="col-sm-12">
    <div class="mb-3">
        <label class="form-label">Descripción Corta</label>
        <textarea class="form-control" id="exampleFormControlTextarea1" rows="3"></textarea>
    </div>
</div><!-- Col -->

{{-- Long Description --}}
<div class="col-sm-12">
    <div class="mb-3">
        <label class="form-label">Descripción Larga</label>
        <textarea class="form-control" name="tinymce" id="tinymceExample" rows="10"></textarea>
    </div>
</div><!-- Col --> 
```
Para soportar el tinymce hay que incluir en resources/views/admin/admin_dashboard.blade.php
```php
{{-- Plugins para soportar al editor tinymce que lo usamos en resources/views/backend/property/add_property.blade.php --}}
<script src="{{ asset('backend/assets/vendors/tinymce/tinymce.min.js') }}"></script>
<script src="{{ asset('backend/assets/js/tinymce.js') }}"></script>
```
Para los check boxes de Features Property y Hot Property insertamos en resources/views/backend/property/add_property.blade.php
```php
{{-- Checkboxes Features Property y Hot Property --}}
<div class="mb-3">

    <div class="form-check form-check-inline">
        <input type="checkbox" name="featured" value="1" class="form-check-input" id="checkInline1">
        <label class="form-check-label" for="checkInline1">
            Features Property
        </label>
    </div>

    <div class="form-check form-check-inline">
        <input type="checkbox" name="hot" value="1" class="form-check-input" id="checkInline">
        <label class="form-check-label" for="checkInline">
            Hot Property
        </label>
    </div>

</div> 
```
En name va el nombre del campo y en value va el valor que inertaremos en el campo si lo dejaron en check!

Listo!

