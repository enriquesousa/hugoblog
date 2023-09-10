---
title: "Manage Property Update Single and Multiple Image Part 1"
date: 2023-05-26T08:55:23-07:00
draft: false
menuTitle: "71. Manage Property Update Single and Multiple Image Part 1"
weight: 71
---

Agregarla sección de poder actualizar la foto thumbnail principal.
En resources/views/backend/property/edit_property.blade.php
```php
{{-- Property Mail Thumbnail Image Update  --}}
<div class="page-content" style="margin-top: -35px">
    <div class="row profile-body">

        <!-- wrapper datos para editar con el total del ancho 12 columnas -->
        <div class="col-md-12 col-xl-12 middle-wrapper">
            <div class="row">

                <div class="card">
                    <div class="card-body">
                        <h6 class="card-title">Editar Imagen Miniatura Principal</h6>

                        <form method="POST" action="{{ route('update.property') }}" id="myForm" enctype="multipart/form-data">
                        @csrf

                            {{-- Para capturar el id del record que queremos editar --}}
                            <input type="hidden" name="id" value="{{ $property->id }}">
                            {{-- Enviar también la imagen vieja para poder borrarla --}}
                            <input type="hidden" name="old_img" value="{{ $property->property_thambnail }}">

                            {{-- Escoger Nueva y Ver Imagen Miniatura Anterior --}}
                            <div class="row mb-3">

                                {{-- Escoger Nueva - picture thumbnail --}}
                                <div class="form-group col-md-6">
                                    <label class="form-label text-warning">Imagen Miniatura</label>
                                    <input type="file" name="property_thambnail" class="form-control"
                                        onChange="mainThamUrl(this)">
                                    <img src="" id="mainThmb">
                                </div>

                                {{-- picture thumbnail Anterior --}}
                                <div class="form-group col-md-6">
                                    <label class="form-label text-warning"></label>
                                    <img src="{{ asset($property->property_thambnail) }}" alt="" style="width: 100px; height: 100px;">
                                </div>

                            </div>

                            <button type="submit" class="btn btn-primary">Salvar Cambios</button>

                        </form>
                    </div>
                </div>

            </div>
        </div>

    </div>
</div> 
```
Creamos otra forma bajo la otra forma para separarlas y sea mas claro
lo que estamos tratando de actualizar.

En la proxima lección terminamos la ruta para que reemplaza la foto vieja por la nueva.
Listo!
