---
title: "Imágenes"
menuTitle: "17. Imágenes"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 25
---

### Especificar Ancho de Imágenes
 Nosotros podemos especificar el ancho que queramos que tengan nuestras imágenes, en el campo "image_url" podemos agregar el siguiente código.
 ```php
 {
    "resize": {
        "width": "500",
        "height": null
    },
    "validation": {
        "add": {
            "rule": "required"
        }
    }
} 
```

Asi podemos controlar el tamaño de las imágenes que almacenamos en nuestro servidor. Esto funciona muy bien para imágenes que sean mayor a el ancho de 500px.

### Upsize Image
Pero que pasa si queremos subir una imagen que sea menor a 500px de ancho, con el código que tenemos ahorita la imagen no la subiría a 500px, si queremos que si la pueda subir, tenemos que agregar el siguiente código "upsize": true, en el su area correspondiente. 

```php
{
    "resize": {
        "width": "500",
        "height": null
    },
    "upsize": true,
    "validation": {
        "add": {
            "rule": "required"
        }
    }
} 
```

### Bajar resolución a imagen
Un sitio popular para bajar el peso de imágenes es https://tinypng.com/ , con Voyager también podemos comprimir nuestras imágenes, utilizando el atributo "quality", por ejemplo las vamos a reducir al 70%, asi:

```php
{
    "resize": {
        "width": "500",
        "height": null
    },
    "quality": "70%",
    "upsize": true,
    "validation": {
        "add": {
            "rule": "required"
        }
    }
} 
```

### Imagen miniatura
Para reducir la imagen a un miniatura, en este caso vamos a crear dos miniaturas una al 50% y otra al 25% en "name" las podemos nombrar como queramos.
```php
{
    "resize": {
        "width": "500",
        "height": null
    },
    "quality": "70%",
    "upsize": true,
    "thumbnails":[
        {
            "name": "medium",
            "scale": "50%"
        },
        {
            "name": "small",
            "scale": "25%"
        },
        {
            "name": "cropped",
            "crop": {
                "width": "300",
                "height": null
            }
        }
    ],
    "validation": {
        "add": {
            "rule": "required"
        }
    }
} 
```
También podemos especificar el ancho y el alto que puede tener la miniatura, con el atributo "crop". Con estos "thumbnails" al momento de subir una imagen, se subirán también estos tres miniaturas que asignamos, con los nombres medium, small y cropped.




