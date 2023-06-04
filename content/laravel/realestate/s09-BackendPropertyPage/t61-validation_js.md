---
title: "Add Property JavaScript Validation"
date: 2023-05-26T08:53:32-07:00
draft: false
menuTitle: "61. Add Property JavaScript Validation"
weight: 61
---

Agragar validacion a nuestra forma!
En resources/views/backend/property/add_property.blade.php
- Empatar la forma con el id="myForm" que ya definimos en el codigo JS al final de la pagina
- No olvidar usar el "form-group" par alos campos que queremos validar
```php
...
{{-- Nombre Propiedad --}}
<div class="col-sm-6">
    <div class="form-group mb-3">
        <label class="form-label">Nombre</label>
        <input type="text" name="property_name" class="form-control">
    </div>
</div><!-- Col -->

{{-- Property Status --}}
<div class="col-sm-6">
    <div class="form-group mb-3">
        <label class="form-label">Estatus</label>
        <select name="property_status" class="form-select"
            id="exampleFormControlSelect1">
            <option selected="" disabled="">Seleccionar Estatus</option>
            <option value="rent">Para Renta</option>
            <option value="buy">Para Compra</option>
        </select>
    </div>
</div><!-- Col -->

{{-- Lowest Price --}}
<div class="col-sm-6">
    <div class="form-group mb-3">
        <label class="form-label">Precio mas Bajo</label>
        <input type="text" name="lowest_price" class="form-control">
    </div>
</div><!-- Col -->

{{-- Max Price --}}
<div class="col-sm-6">
    <div class="form-group mb-3">
        <label class="form-label">Precio mas Alto</label>
        <input type="text" name="max_price" class="form-control">
    </div>
</div><!-- Col --> 
...
{{-- Property Type --}}
<div class="col-sm-4">
    <div class="form-group mb-3">
        <label class="form-label">Tipo de Propiedad</label>
        <select name="ptype_id" class="form-select" id="exampleFormControlSelect1">
            <option selected="" disabled="">Seleccionar Tipo</option>
            @foreach($propertytype as $ptype)
                <option value="{{ $ptype->id }}">{{ $ptype->type_name }}</option>
            @endforeach
        </select>
    </div>
</div><!-- Col -->
...
{{-- Script de JS para la Validación --}}
<script type="text/javascript">
    $(document).ready(function (){
        $('#myForm').validate({

            rules: {
                property_name: {
                    required : true,
                },
                property_status: {
                    required : true,
                },
                lowest_price: {
                    required : true,
                },
                max_price: {
                    required : true,
                },
                ptype_id: {
                    required : true,
                },

            },

            messages :{

                property_name: {
                    required : 'Favor entrar nombre de la Propiedad, campo requerido',
                },
                property_status: {
                    required : 'Favor seleccionar Estatus de la Propiedad, campo requerido',
                },
                lowest_price: {
                    required : 'Favor entrar precio mas bajo, campo requerido',
                },
                max_price: {
                    required : 'Favor entrar precio mas alto, campo requerido',
                },
                ptype_id: {
                    required : 'Favor seleccionar Tipo de Propiedad, campo requerido',
                },

            },

            errorElement : 'span',
            errorPlacement: function (error,element) {
                error.addClass('invalid-feedback');
                element.closest('.form-group').append(error);
            },
            highlight : function(element, errorClass, validClass){
                $(element).addClass('is-invalid');
            },
            unhighlight : function(element, errorClass, validClass){
                $(element).removeClass('is-invalid');
            },
        });
    });

</script>
```

Listo!