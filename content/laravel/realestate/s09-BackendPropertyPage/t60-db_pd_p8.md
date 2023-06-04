---
title: "Add Property Database and Page Design Part 8"
date: 2023-05-26T08:53:24-07:00
draft: false
menuTitle: "60. Add Property Database and Page Design Part 8"
weight: 60
---

Agregar las Instalaciones Cercas en resources/views/backend/property/add_property.blade.php
```php
...
{{-- Facilities Option --}}
<div class="row add_item">

    {{-- Instalaciones Cercanas --}}
    <div class="col-md-4">
        <div class="mb-3">
            <label for="facility_name" class="form-label">Instalaciones Cercanas</label>
            <select name="facility_name[]" id="facility_name" class="form-control">
                <option value="">Selecciona Instalación</option>
                <option value="Hospital">Hospital</option>
                <option value="Super Mercado">Super Mercado</option>
                <option value="Escuela">Escuela</option>
                <option value="Entretenimiento">Entretenimiento</option>
                <option value="Farmacia">Farmacia</option>
                <option value="Aeropuerto">Aeropuerto</option>
                <option value="Estación de Tren">Estación de Tren</option>
                <option value="Parada de autobus">Parada de autobus</option>
                <option value="Playa">Playa</option>
                <option value="Centro Comercial">Centro Comercial</option>
                <option value="Banco">Banco</option>
                <option value="Cine">Cine</option>
                <option value="Restaurante">Restaurante</option>
            </select>
        </div>
    </div>

    {{-- Distance --}}
    <div class="col-md-4">
        <div class="mb-3">
            <label for="distance" class="form-label">Distancia</label>
            <input type="text" name="distance[]" id="distance" class="form-control" placeholder="Distancia en (Km)">
        </div>
    </div>

    {{-- Add More.. --}}
    <div class="form-group col-md-4" style="padding-top: 30px;">
        <a class="btn btn-success addeventmore"><i class="fa fa-plus-circle"></i>Agregar mas...</a>
    </div>

</div>
<!---end row--> 
...
<!--========== Start of add multiple class with ajax, Para agregar mas opciones a Facilities Option ==============-->
<div style="visibility: hidden">
    <div class="whole_extra_item_add" id="whole_extra_item_add">
        <div class="whole_extra_item_delete" id="whole_extra_item_delete">
            <div class="container mt-2">
                <div class="row">

                    <div class="form-group col-md-4">
                        <label for="facility_name">Facilities</label>
                        <select name="facility_name[]" id="facility_name" class="form-control">

                            <option value="">Selecciona Instalación</option>
                            <option value="Hospital">Hospital</option>
                            <option value="Super Mercado">Super Mercado</option>
                            <option value="Escuela">Escuela</option>
                            <option value="Entretenimiento">Entretenimiento</option>
                            <option value="Farmacia">Farmacia</option>
                            <option value="Aeropuerto">Aeropuerto</option>
                            <option value="Estación de Tren">Estación de Tren</option>
                            <option value="Parada de autobus">Parada de autobus</option>
                            <option value="Playa">Playa</option>
                            <option value="Centro Comercial">Centro Comercial</option>
                            <option value="Banco">Banco</option>
                            <option value="Cine">Cine</option>
                            <option value="Restaurante">Restaurante</option>

                        </select>
                    </div>
                    <div class="form-group col-md-4">
                        <label for="distance">Distancia</label>
                        <input type="text" name="distance[]" id="distance" class="form-control"
                            placeholder="Distancia en (Km)">
                    </div>
                    <div class="form-group col-md-4" style="padding-top: 20px">
                        <span class="btn btn-success btn-sm addeventmore"><i class="fa fa-plus-circle">Agregar</i></span>
                        <span class="btn btn-danger btn-sm removeeventmore"><i class="fa fa-minus-circle">Remover</i></span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</div>

<!----For Section-------->
<script type="text/javascript">
    $(document).ready(function(){
       var counter = 0;
       $(document).on("click",".addeventmore",function(){
             var whole_extra_item_add = $("#whole_extra_item_add").html();
             $(this).closest(".add_item").append(whole_extra_item_add);
             counter++;
       });
       $(document).on("click",".removeeventmore",function(event){
             $(this).closest("#whole_extra_item_delete").remove();
             counter -= 1
       });
    });
</script>
<!--========== End of add multiple class with ajax ==============-->
```
Con esto logramos poder poder agregar Instalciones cercas y todas las podremos grabar a la tabla, esto
se vera mas adelante.

Listo!
