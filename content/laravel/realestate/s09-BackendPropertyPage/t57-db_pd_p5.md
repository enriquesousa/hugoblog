---
title: "Add Property Database and Page Design Part 5"
date: 2023-05-26T08:53:01-07:00
draft: false
menuTitle: "57. Add Property Database and Page Design Part 5"
weight: 57
---

Seguir diseñando la forma en resources/views/backend/property/add_property.blade.php
```php
<form>

    {{-- Row 1 --}}
    <div class="row">

        {{-- Nombre Propiedad --}}
        <div class="col-sm-6">
            <div class="mb-3">
                <label class="form-label">Nombre</label>
                <input type="text" name="property_name" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- Property Status --}}
        <div class="col-sm-6">
            <div class="mb-3">
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
            <div class="mb-3">
                <label class="form-label">Precio mas Bajo</label>
                <input type="text" name="lowest_price" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- Max Price --}}
        <div class="col-sm-6">
            <div class="mb-3">
                <label class="form-label">Precio mas Alto</label>
                <input type="text" name="max_price" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- picture thumbnail --}}
        <div class="col-sm-6">
            <div class="mb-3">
                <label class="form-label">Imagen Miniatura</label>
                <input type="file" name="property_thambnail" class="form-control"
                    onChange="mainThamUrl(this)">
                <img src="" id="mainThmb">
            </div>
        </div><!-- Col -->

        {{-- Multiple Image --}}
        <div class="col-sm-6">
            <div class="mb-3">
                <label class="form-label">Imágenes Multiples</label>
                <input type="file" name="multi_img[]" class="form-control" id="multiImg"
                    multiple="">
                <div class="row" id="preview_img"> </div>
            </div>
        </div><!-- Col -->

    </div><!-- Row -->

    {{-- Row 2 --}}
    <div class="row">

        {{-- BedRooms --}}
        <div class="col-sm-3">
            <div class="mb-3">
                <label class="form-label">Dormitorios</label>
                <input type="text" name="bedrooms" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- Bathrooms --}}
        <div class="col-sm-3">
            <div class="mb-3">
                <label class="form-label">Baños</label>
                <input type="text" name="bathrooms" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- Garage --}}
        <div class="col-sm-3">
            <div class="mb-3">
                <label class="form-label">Cochera</label>
                <input type="text" name="garage" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- Garage Size --}}
        <div class="col-sm-3">
            <div class="mb-3">
                <label class="form-label">Tamaño de Cochera</label>
                <input type="text" name="garage_size" class="form-control">
            </div>
        </div><!-- Col -->

    </div><!-- Row -->

    {{-- Row 3 --}}
    <div class="row">

        {{-- Address --}}
        <div class="col-sm-3">
            <div class="mb-3">
                <label class="form-label">Dirección</label>
                <input type="text" name="address" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- City --}}
        <div class="col-sm-3">
            <div class="mb-3">
                <label class="form-label">Ciudad</label>
                <input type="text" name="city" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- State --}}
        <div class="col-sm-3">
            <div class="mb-3">
                <label class="form-label">Estado</label>
                <input type="text" name="state" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- Postal Code --}}
        <div class="col-sm-3">
            <div class="mb-3">
                <label class="form-label">Código Postal</label>
                <input type="text" name="postal_code" class="form-control">
            </div>
        </div><!-- Col -->

    </div><!-- Row -->

    {{-- Row 4 --}}
    <div class="row">

        {{-- Property Size --}}
        <div class="col-sm-4">
            <div class="mb-3">
                <label class="form-label">Tamaño Propiedad</label>
                <input type="text" name="property_size"  class="form-control" >
            </div>
        </div><!-- Col -->

        {{-- Property Video --}}
        <div class="col-sm-4">
            <div class="mb-3">
                <label class="form-label">Video Propiedad</label>
                <input type="text" name="property_video"  class="form-control" >
            </div>
        </div><!-- Col -->

        {{-- Neighborhood --}}
        <div class="col-sm-4">
            <div class="mb-3">
                <label class="form-label">Vecindario</label>
                    <input type="text" name="neighborhood"  class="form-control" >
            </div>
        </div><!-- Col -->

    </div><!-- Row -->

    {{-- Row 5 --}}
    <div class="row">

        {{-- Latitude --}}
        <div class="col-sm-6">
            <div class="mb-3">
                <label class="form-label">Latitude</label>
                <input type="text" name="latitude" class="form-control">
                <a href="https://www.latlong.net/convert-address-to-lat-long.html" target="_blank">Go here to get Latitude
                    from address</a>
            </div>
        </div><!-- Col -->

        {{-- Longitude --}}
        <div class="col-sm-6">
            <div class="mb-3">
                <label class="form-label">Longitude</label>
                <input type="text" name="longitude" class="form-control">
                <a href="https://www.latlong.net/convert-address-to-lat-long.html" target="_blank">Go here to get Longitude
                    from address</a>
            </div>
        </div><!-- Col -->

    </div><!-- Row -->

    {{-- Row 6 --}}
    <div class="row">

        {{-- Property Type --}}
        <div class="col-sm-4">
            <div class="mb-3">
                <label class="form-label">Property Type</label>
                <input type="text" name="property_size" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- Property Amenities --}}
        <div class="col-sm-4">
            <div class="mb-3">
                <label class="form-label">Property Amenities</label>
                <input type="text" name="property_video" class="form-control">
            </div>
        </div><!-- Col -->

        {{-- Agent --}}
        <div class="col-sm-4">
            <div class="mb-3">
                <label class="form-label">Agent</label>
                <input type="text" name="neighborhood" class="form-control">
            </div>
        </div><!-- Col -->

    </div><!-- Row -->

</form> 
```
Dejamos par ala siguiente lección el {{-- Row 6 --}}
Ya que estos campos los tenemos que relacionar con nuestras otras tablas.

Listo!
