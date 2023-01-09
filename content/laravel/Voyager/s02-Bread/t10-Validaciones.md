---
title: "Validaciones"
menuTitle: "10. Validaciones"
date: 2022-12-24T07:47:01-08:00
draft: false
weight: 35
---


### Validar un campo
Entrar a Tools/BREAD y dirigirnos a nuestra tabla **products** y luego a **Editar**, escoger un campo por ejemplo **name**, ir a la ultima columna (**Detalles opcionales**) y colocar nuestro código de validación ahi, asignar un atributo que se llame **validation** y asignarle un objeto en donde ya podemos asignar las reglas asi.

```php
{
    "validation": {
        "rule": "required",
        "messages": {
            "required": "El campo nombre es obligatorio"
        }
    }
}
```

Lo mismo podemos hacer para los demás campos.

### Personalizar las reglas
Por ejemplo, que el valor mínimo del campo nombre al agregar un nuevo registro sea de 10 caracteres y cuando vayamos a editar el campo dar la regla de un mínimo de 20, esto es solo como ejemplo.

```php
{
    "validation": {
        "rule": "required",
        "add": {
            "rule": "min:10"
        },
        "edit": {
            "rule": "min:20"
        }
    }
} 
```

Vemos como podemos dar dos reglas diferentes, una cuando queremos agregar un nuevo producto y otra cuando intentamos editar un producto.

