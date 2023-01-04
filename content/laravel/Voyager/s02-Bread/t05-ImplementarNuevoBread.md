---
title: "Implementar Nuevo Bread"
menuTitle: "05. Implementar Nuevo Bread"
date: 2022-12-24T07:47:01-08:00
draft: false
weight: 10
---

### Paso 1
*Opción 1*:
Dentro del side menu de Voyager ir a Tools/Database, si vemos nuestra nueva tabla "products", ahi tenemos la opción de "Añadir BREAD a esta tabla".

*Opción 2*:
Dentro del side menu de Voyager ir a Tools/BREAD, ahi podemos también "Añadir BREAD a esta tabla".

**Que es Bread?**
- **BROWSE** (field will show up when you browse the current data)
- **READ** (field will show when you click to view the current data)
- **EDIT** (field will be visible and allow you to edit the data)
- **ADD** (field will be visible when you choose to create a new data type)
- *DELETE* (doesn't pertain to delete so this can be checked or unchecked)

### Paso 2
Llenamos en formulario.

![Widgets](/Voyager/products_bread1.png)

Para asignar el icono le damos click a "clase de Voyager Fonts" y abrimos en un nuevo tab, ahi podemos escoger el icono que mas nos guste, solo copiar el nombre y después pegarlo ya en el campo icono del formulario.

Le damos click a "Enviar".
Listo!

Ahora ya podemos ver utilizar nuestro BREAD en el side menu, hasta abajo vemos ya el menu de "Productos".
Vemos como en la url:
```php
 http://voyager.lndo.site/admin/products
```
ya utilizar el *slug* ("products") que le asignamos en el formulario.

