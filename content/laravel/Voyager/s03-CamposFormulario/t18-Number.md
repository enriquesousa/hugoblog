---
title: "Number"
menuTitle: "18. Number"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 30
---

### Campo numérico
Por el momento el único campo que tenemos que es numérico es id, asi que poner en el tipo de entrada "Number".

Imaginemos que queramos tener un campo de tipo "Float" y queremos incluir los decimales, 2 decimales , con un valor numérico que mínimo puede ser 1, hasta un valor máximo de 100, y valor por defecto 1, aquí esta el código para poder hacer eso.
```php
{
    "step": 0.01,
    "min": 1,
    "max": 100,
    "default": 1
}
```

