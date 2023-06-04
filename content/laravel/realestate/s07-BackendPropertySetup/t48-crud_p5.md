---
title: "Property Type Crud Part 5"
date: 2023-05-26T08:50:46-07:00
draft: false
menuTitle: "48. Property Type Crud Part 5"
weight: 48
---
Delete y aviso con Sweet Alert 2
- En resources/views/admin/admin_dashboard.blade.php
```php
{{-- Plugin for sweet alert 2 --}}
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@10"></script>
<script src="{{ asset('backend/assets/js/code/code.js') }}"></script> 
```
- Crear public/backend/assets/js/code/code.js
```php
$(function () {
    $(document).on("click", "#delete", function (e) {
        e.preventDefault();
        var link = $(this).attr("href");

        Swal.fire({
            title: "Are you sure?",
            text: "Delete This Data?",
            icon: "warning",
            showCancelButton: true,
            confirmButtonColor: "#3085d6",
            cancelButtonColor: "#d33",
            confirmButtonText: "Yes, delete it!",
        }).then((result) => {
            if (result.isConfirmed) {
                window.location.href = link;
                Swal.fire("Deleted!", "Your file has been deleted.", "success");
            }
        });
    });
}); 
```
Y en resources/views/backend/type/all_type.blade.php usar el id="delete"
```php
<td>
    <a href="{{ route('edit.type',$item->id) }}" class="btn btn-inverse-warning">Editar</a>
    <a href="{{ route('delete.type',$item->id) }}" class="btn btn-inverse-danger" id="delete">Borrar</a>
</td>
```
Ahora ya podemos usar nuestro mensaje con sweet alert 2.
Para modificar los mensajes solo editarlos en public/backend/assets/js/code/code.js

En routes/web.php
```php
Route::get('/delete/type/{id}', 'DeleteType')->name('delete.type'); 
```

En app/Http/Controllers/Backend/PropertyTypeController.php
```php
// Delete Type
public function DeleteType($id){

    PropertyType::findOrFail($id)->delete();

    $notification = array(
        'message' => 'Property Type se elimino con éxito!',
        'alert-type' => 'success'
    );

    return redirect()->back()->with($notification);

} 
```
Listo!

