---
title: "Show Image Using Javascript"
date: 2023-05-26T08:52:50-07:00
draft: false
menuTitle: "56. Show Image Using Javascript"
weight: 56
---

Usar los scrips de JS para visualizar tanto single image como multiple images.
en resources/views/backend/property/add_property.blade.php
```php
...
{{-- picture thumbnail --}}
<div class="col-sm-6">
    <div class="mb-3">
        <label class="form-label">Imagen Miniatura</label>
        <input type="file" name="property_thambnail" class="form-control" onChange="mainThamUrl(this)"  >
        <img src="" id="mainThmb">
    </div>
</div><!-- Col -->

{{-- Multiple Image --}}
<div class="col-sm-6">
    <div class="mb-3">
        <label class="form-label">Imágenes Multiples</label>
        <input type="file" name="multi_img[]" class="form-control" id="multiImg" multiple="">
        <div class="row" id="preview_img"> </div>
    </div>
</div><!-- Col --> 
...
{{-- Script de JS para visualizar la imagen --}}
<script type="text/javascript">
    function mainThamUrl(input){
        if (input.files && input.files[0]) {
            var reader = new FileReader();
            reader.onload = function(e){
              $('#mainThmb').attr('src',e.target.result).width(80).height(80);
            };
            reader.readAsDataURL(input.files[0]);
        }
    }
</script>

{{-- Script de JS para visualizar multiples imágenes --}}
<script type="text/javascript">

    $(document).ready(function(){
        $('#multiImg').on('change', function(){ //on file input change
        if (window.File && window.FileReader && window.FileList && window.Blob) //check File API supported browser
        {
            var data = $(this)[0].files; //this file data

            $.each(data, function(index, file){ //loop though each file
                if(/(\.|\/)(gif|jpe?g|png|webp)$/i.test(file.type)){ //check supported file type
                    var fRead = new FileReader(); //new filereader
                    fRead.onload = (function(file){ //trigger function on successful read
                    return function(e) {
                        var img = $('<img/>').addClass('thumb').attr('src', e.target.result) .width(100)
                    .height(80); //create image element
                        $('#preview_img').append(img); //append image to output element
                    };
                    })(file);
                    fRead.readAsDataURL(file); //URL representing the file's data.
                }
            });

        }else{
            alert("Your browser doesn't support File API!"); //if File API is absent
        }
        });
    });

</script>
```
Listo!
