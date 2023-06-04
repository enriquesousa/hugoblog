---
title: "Add Property Database and Page Design Part 2"
date: 2023-05-26T08:52:42-07:00
draft: false
menuTitle: "55. Add Property Database and Page Design Part 2"
weight: 55
---

Diseño de la forma de captura de datos en resources/views/backend/property/add_property.blade.php
```php
Nos quedamos hasta aqui:
{{-- picture thumbnail --}}
<div class="col-sm-6">
    <div class="mb-3">
        <label class="form-label">Imagen Miniatura</label>
        <input type="file" name="lowest_price" class="form-control">
    </div>
</div><!-- Col -->

{{-- Multiple Image --}}
<div class="col-sm-6">
    <div class="mb-3">
        <label class="form-label">Imágenes Multiples</label>
        <input type="file" name="lowest_price" class="form-control">
    </div>
</div><!-- Col --> 
```
En la siguiente lección implementaremos la selección de la imágenes!

Listo!
