---
title: "Personalizar Campos"
menuTitle: "13. Personalizar Campos"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 5
---

### Nota
Cambiar todas las reglas de validación de los campos para que solo sea requerido el campo.

En los campos "name", "description", y "user_id" dejar solo el siguiente código.
```php
{
    "validation": {
        "rule": "required"
    }
} 
```

Y solo para el campo "image_url" dejar la regla que es requerido solo en el caso de añadir una nueva imagen, asi:
```php
{
    "validation": {
        "add":{
            "rule": "required"
        }
    }
}
```
Esto lo hacemos para que el usuario pueda editar u producto sin necesidad de mandar una nueva imagen. 

### Agregar descripción a cada uno de los campos del formulario
Para agregar una descripción al campo name.
```php
{
    "validation": {
        "rule": "required"
    },
    "description": "Este es un mensaje para mi yo del futuro"
} 
```
Si enviamos los cambios y recargamos pa pagina de crear nuevo producto, vemos que ya aparece un pequeño símbolo de interrogación abajo del campo nombre, en el cual si hacemos hover sobre el, veremos nuestra descripción.

Ahora si queremos manipular el espacio que ocupa nuestro campo en el diseño de la pagina, hay que tener en cuenta que Voyager utiliza Bootstrap y por el momento el campo de nam esta ocupando las 12 columnas disponibles del grid de diseño, para modificar esto y hacer mas chico, tenemos que modificar un atributo mas "display" en los detalles opcionales del campo.
```php
{
    "validation": {
        "rule": "required"
    },
    "description": "Este es un mensaje para mi yo del futuro",
    "display": {
        "width": 6
    }
} 
```
Y lo mismo podríamos agregar a cualquier otro campo.

### Asignar valor por defecto a un campo
Usar el atributo "default".
Ejemplo par ael campo name:
```php
{
    "validation": {
        "rule": "required"
    },
    "description": "Este es un mensaje para mi yo del futuro",
    "default": "Este es un valor por defecto"
} 
```

### Añadir campo slug a la tabla products
En el side bar nos vamos os a Base de Datos Editar tabla products y "+Añadir Columna".
![Widgets](/Voyager/añadir-campo-slug.png)

Ahora ir al BREAD Editar y mover el campo slug justo abajo del nombre y especificar con los chk marks que solo se va poder Añadir y Editar y de damos "Enviar".
Para lograr que el slug se genere automáticamente, poner el siguiente atributo en la columna de sus "Detalles opcionales".
```php
{
    "slugify": {
        "origin": "name",
        "forceUpdate": true
    }
}
```    
Listo!
con esto ahora cada vez que se cree un nuevo producto, también se formara su slug de manera automática, en tiempo real vemos como conforme vamos llenando el campo de name se va colocando en letras minúsculas y los espacios los va transformando en guiones en el campo slug.
