---
title: "SweetAlert2"
date: 2024-05-17T07:11:25-07:00
draft: false
---

## Objetivo
Instalar sweet Alert 2.
![sweetAlert2](/laraveltips/sweetalert2-1.png)
<br>

## Ejemplo 
Para el botón dele de proveedores.

## Ruta en Vista
Crear ruta en vista.
`En resources/views/backend/supplier/list_supplier.blade.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
{{-- Acciones --}}
<td>
	{{-- Edit --}}
	<a href="{{ route('edit.supplier', $item->id) }}" class="btn btn-info sm"
		title="Edit Data"><i class="fas fa-edit"></i></a>

	{{-- Details --}}
	<a href="{{ route('show.supplier', $item->id) }}" class="btn btn-success sm"
		title="Details Data"><i class="fas fa-eye"></i></a>

	{{-- Delete --}}
	<a href="{{ route('delete.supplier', $item->id) }}" class="btn btn-danger sm" title="Delete Data"
		id="delete"><i class="fas fa-trash-alt"></i></a>
</td>
```
</details>
<br>

## Web Route
Crear ruta.
`En routes/web.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
Route::controller(SupplierController::class)->group(function () {
    Route::get('/list/supplier', 'ListSupplier')->name('list.supplier');
    Route::get('/add/supplier', 'AddSupplier')->name('add.supplier');
    Route::post('/store/supplier', 'StoreSupplier')->name('store.supplier');
    Route::get('/edit/supplier/{id}', 'EditSupplier')->name('edit.supplier');
    Route::post('/update/supplier/{id}', 'UpdateSupplier')->name('update.supplier');
    Route::get('/show/supplier/{id}', 'ShowSupplier')->name('show.supplier');
    Route::get('/delete/supplier/{id}', 'DeleteSupplier')->name('delete.supplier');
});
```
</details>
<br>

## Controlador
Crear método.
`En app/Http/Controllers/Pos/SupplierController.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
// DeleteSupplier
public function DeleteSupplier($id){

	Supplier::findOrFail($id)->delete();

	$notification = array(
		'message' => 'Proveedor Eliminado Correctamente',
		'alert-type' => 'info'
	);

	return redirect()->back()->with($notification);
}
```
</details>
<br>

## Sweet Alert 2
Instalar sweet alert 2, copiar este código antes de que termine el body.
`En resources/views/admin/admin_master.blade.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
{{-- Sweetalert, para mensajes de confirmación en botones de Eliminar en vista --}}
<script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>
<script src="{{ asset('backend/assets/js/code.js') }}"></script>
```
</details>
<br>

Ahora copiar el siguiente código, crear un archivo que se llame **code.js**.
`En public/backend/assets/js/code.js`
<details>
  <summary>Click para ver Código. </summary>
  
```
$(function () {
    $(document).on('click', '#delete', function (e) {
        e.preventDefault();
        var link = $(this).attr("href");


        Swal.fire({
            title: 'Estas seguro?',
            text: "Que deseas Eliminar este registro!",
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText: 'Si, Eliminar',
        }).then((result) => {
            if (result.isConfirmed) {
                window.location.href = link
                Swal.fire(
                    'Eliminado!',
                    'El registro ha sido eliminado.',
                    'success'
                )
            }
        })


    });

});
```
</details>
<br>

No olvidar colocar el `id="delete"` en la clase del botón **delete**.
`En resources/views/backend/supplier/list_supplier.blade.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
{{-- Delete --}}
<a href="{{ route('delete.supplier', $item->id) }}" class="btn btn-danger sm" title="Delete Data"
	id="delete"><i class="fas fa-trash-alt"></i></a>
```
</details>
<br>


* * *
Listo!
