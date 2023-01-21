---
title: "Checkbox"
menuTitle: "14. Checkbox"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 10
---

### Agregar campo adicional checkbox  
Vamos a agregar una campo adicional a a tabla "products" con el objetivo de que nos ayude a determinar si dicho producto se debe de publicar en nuestra tienda o no (true or false).

**Paso 1: Generar campo en Base de Datos**
Generamos el campo en la base de datos con Tools Database Editar Añadir Columna. Como no existe en mysql un campo de tipo booleano lo que hacemos es poner el tipo de "TINYINT" y de longitud "1", lo que estaremos almacenando en realidad el un 1 para true y un 0 para false, al terminar "Actualizar tabla".

**Paso 2: Editar Atributos**
BREAD de productos Editar, y en poner los siguientes atributos:
![Widgets](/Voyager/campo_active.png)

**Paso 3: Personalizar nombres**
Personalizar nombres del botón.
En "Tools BREAD Editar" los detalles opcionales de los atributos del campo "active" modificarlo asi:
```php
{
    "on": "Activo",
    "off": "Inactivo"
} 
```
Ahora si nos vamos a "Tools BREAD Navegar" vemos ue ya cambio el nombre que imprime en la tabla, ya no muestra los valores de "0" o "1" ahora ya muestra "Activo" o "Inactivo", y si ademas entramos a editar uno de los productos vemos que también ya cambio el nombre del botón cuando lo movemos activar o desactivar.

**Paso 4: Valor por default**
Al "Crear" un muevo producto y queremos que el campo checkbox tenga un valor predeterminado, esto lo hacemos con la propiedad "checked" en los atributos del campo asi:
```php
{
    "on": "Activo",
    "off": "Inactivo",
    "checked": true
}
```

**Paso 5: Multiple Checkbox**
Para poder mostrar multiples checkbox, por ejemplo para mostrar 3 checkbox con los nombres:
1 - Opción 1
2 - Opción 2
3 - Opción 3

Hacemos lo siguiente:
```php
{
    "options":{
        "1": "Opción 1",
        "2": "Opción 2",
        "3": "Opción 3"
    }
}
```
![Widgets](/Voyager/multiplecheckbox.png)

**Paso 6: Controlador Personalizado**
Para tener mas control sobre las acciones del multiple checkbox podemos crear nuestro propio controlador.
```php
lando php artisan make:controller Voyager/ProductController 
```

Para no crear desde cero todo el controlador, tomamos como base el controlador base de Voyager, incluyendo en el controlador:
```php
use TCG\Voyager\Http\Controllers\VoyagerBaseController;

class ProductController extends VoyagerBaseController
{
    //
}
```
Nota como cambiamos a que extienda el controlador en vez de a Controller a VoyagerBaseController, al hacer esto ya tenemos disponibles todos los métodos que incluye VoyagerBaseController como lo son:
```php
index()
show()
edit()
update()
create()
store()
destroy()
restore()
remove_media()
cleanup()
deleteBreadImages()
order()
update_order()
action()
relation()
findSearchableRelationshipRow()
getSortableColumns()
relationIsUsingAccessorAsLabel()
```  
Todos estos son los métodos que utiliza Voyager para poder trabajar correctamente.

Para decirle a Voyager que ahora estaremos utilizando nuestro propio controlador personalizado "ProductController", en "BREAD Products Edit" agregar el nombre del controlador "App\Http\Controllers\Voyager\ProductController" en: 
app/Http/Controllers/Voyager/ProductController.php
![Widgets](/Voyager/añadirProductControllerName.png)

Vamos a sobre escribir el método store()
Para ver los valores que me regresa la forma crear nuevo producto.
```php
public function store(Request $request)
{
    return $request->all();
} 
```
Esto fue solo un ejemplo.

Regresa a solo un checkbox y borrar este método y dejar los atributos como estaban para un solo check box. 

**Paso 6: Radio Buttons**
Para crear radio buttons ahora escojamos "Radio Button" en los atributos e insertamos el siguiente código en los detalles opcionales. 
```php
{
    "default": "1",
    "options":{
        "0": "Inactivo",
        "1": "Activo"
    }
} 
```

Listo!

Volver a colocarlo en un solo checkbox con su código correspondiente porque asi es como vamos a trabajar en este proyecto.

