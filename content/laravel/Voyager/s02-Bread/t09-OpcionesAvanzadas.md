---
title: "Opciones Avanzadas (relaciones)"
menuTitle: "09. Opciones Avanzadas (relaciones)"
date: 2022-12-24T07:47:01-08:00
draft: false
weight: 30
---

### Convenciones para nombres
Si no seguimos las convenciones para nombres de las tablas, modelos y llaves foráneas, podemos especificar en detalles de la relación el nombre especifico que le queremos dar a nuestra llave foránea, en este caso podemos darle el mismo nombre que ya le dimos. 

```php
{
    "foreign_pivot_key": "product_id",
    "related_pivot_key": "tag_id"
}
```

Si tu llave primaria no fuera id, entonces tenemos que especificar una:

```php
{
    "foreign_pivot_key": "product_id",
    "related_pivot_key": "tag_id",
    "parent_key": "id"
}
```

### Mostrar tags en orden alfabético

Si nos vamos al side bar Producto y no dirigimos a editar alguno de los productos, y vemos el orden que tienen los tags.

```php
{
    "foreign_pivot_key": "product_id",
    "related_pivot_key": "tag_id",
    "parent_key": "id",
    "sort": {
        "field": "name",
        "direction": "asc"
    }
}
```


