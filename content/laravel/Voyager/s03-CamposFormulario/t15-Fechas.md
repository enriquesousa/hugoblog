---
title: "Fechas"
menuTitle: "15. Fechas"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 15
---

### Cambiar el formato fechas
Para cambiar el formato en el que se despliegan las fechas, nos vamos a los atributos del campo de "created_at" y modificamos asi:
```php
{
    "format": "%d-%m-%Y"
} 
```
Con %Y nos despliega el año con los 4 caracteres "2023", si lo ponemos en minúscula los lo deja en 2 caracteres "23".

Listo!

