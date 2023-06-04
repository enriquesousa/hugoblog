---
title: "Add Validation Message"
date: 2023-05-26T08:51:34-07:00
draft: false
menuTitle: "50. Add Validation Message"
weight: 50
---

Ahora vamos hacer la validación del campo con Java Script
- Copiar validate.min.js a /home/enrique/Sites/realestate/public/backend/assets/js/code
Para soportar este nuevo código tenemos que agregarlo a resources/views/admin/admin_dashboard.blade.php:
```php
{{-- Plugin para hacer validación de campos con JS --}}
<script src="{{ asset('backend/assets/js/code/validate.min.js') }}"></script> 
```
Y agregar este código JS en la pagina donde vamos a usar la validación, en este caso en resources/views/backend/amenities/add_amenities.blade.php, insertarlo antes de que termine la sección:
```php
<script type="text/javascript">
    $(document).ready(function (){
        $('#myForm').validate({
            rules: {
                field_name: {
                    required : true,
                }, 
                
            },
            messages :{
                field_name: {
                    required : 'Please Enter FieldName',
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

@endsection 
```
Asegurarnos de incluir también el código Jquery al inicio de la sección:
```php
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.4/jquery.min.js"></script> 
```
Para que podamos usar ejecutar el código JS.
Agregar el id="myForm" correspondiente a nuestra forma:
```php
<form id="myForm" method="POST" action="{{ route('store.type') }}" class="forms-sample">
@csrf 
```
La forma y el codigo js ya modificados queda asi:
```php
...
<form id="myForm" method="POST" action="{{ route('store.type') }}" class="forms-sample">
@csrf


    {{-- Type Name --}}
    <div class="form-group mb-3">
        <label for="amenities_name" class="form-label">Amenities Name</label>
        <input type="text" name="amenities_name" class="form-control">
    </div>

    <button type="submit" class="btn btn-primary me-2">Save Changes</button>

</form> 
...
<script type="text/javascript">
    $(document).ready(function (){
        $('#myForm').validate({

            rules: {

                amenities_name: {
                    required : true,
                },

            },
            messages :{
                amenities_name: {
                    required : 'Favor entrar nombre de amenities, campo requerido',
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
La ventaja que tenemos al usar la validación con JS es que sin tener que recargar la pagina
los errores de validación son desplegados.
Listo!

