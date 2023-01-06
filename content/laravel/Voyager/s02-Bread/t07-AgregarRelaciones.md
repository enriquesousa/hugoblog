---
title: "Agregar Relaciones"
menuTitle: "07. Agregar Relaciones"
date: 2022-12-24T07:47:01-08:00
draft: false
weight: 20
---


En http://voyager.lndo.site/admin/bread/products/edit ir al final al botón que dice "Crear una relación".

### Paso 1
Crear una relación de tipo: **Belongs To**

### Paso 2
En Category escoger el modelo con el cual lo quiero relacionar, en este caso queremos una relación con el modelo **"User"**.
En el recuadro de la derecha especificamos donde se encuentra este modelo: **"App\Models\User"**.

### Paso 3
Escoger la llave foránea: **"user_id"**

### Paso 4
Después escoger que campo queremos mostrar del usuario: **"name"**

### Paso 5
Por ultimo escoger que valor de user queremos almacenar en user_id, aquí tenemos que escoger: **"id"**  
![Widgets](/Voyager/crear_relacion_products_user.png)

Dar click en el botón "Crear relación"

Al final del BREAD de nuestra tabla "products" vemos ya la relación.
![Widgets](/Voyager/relacion_products_user.png)

Ahora si nos vamos a la forma de crear nuevo producto (http://voyager.lndo.site/admin/products/create) ya aparece la relación con user. 
Para empezar a diseñar nuestra nueva forma, primero hay que capturar un nuevo usuario, en el sidebar escoger **User** y **Crear** uno nuevo.

Nombre: Ivonne Rodriguez
Email: ivonne@gmail.com
Contraseña: 1234
Rol Predeterminado: Administrador
Roles Adicionales:
Idioma: es

Dar el boton de **"Guardar"**

Ahora dar de Alta un producto cualquiera y guardarlo.

Ahora va a hacia:
vendor/tcg/voyager/src/Http/Controllers/VoyagerBaseController.php
Este es el controlador base que se extiende para todos los controladores que se creen para hacer los cruds y tienen todos los métodos necesarios para hacer esta tarea, como lo son:

index() - Browse our Data Type (B)READ
show() - Read an item of our Data Type B(R)EAD
edit() - Edit an item of our Data Type BR(E)AD
update() - POST BR(E)AD
create() - Add a new item of our Data Type BRE(A)D
store() - POST BRE(A)D - Store data
destroy() - Delete an item BREA(D)
restore() -
remove_media() - Delete uploaded file
cleanup() - Remove translations, images and files related to a BREAD item
deleteBreadImages() - Delete all images related to a BREAD item
order() - Order BREAD items
update_order()
action()
relation() - Get BREAD relations data
findSearchableRelationshipRow()
getSortableColumns()
relationIsUsingAccessorAsLabel()

**Listo!**

