---
title: "Tagging"
menuTitle: "08. Tagging"
date: 2022-12-24T07:47:01-08:00
draft: false
weight: 25
---

### Crear Tabla Tags

Crear una nueva tabla, ir a Database y click botón "Crear nueva tabla" y nombrarla "**tags**" también generar modelo "**Si, por favor**", agregar campo "**name**", y también añadir los dos campo de time stamp con el botón "**Añadir Timestamp**".

![Widgets](/Voyager/CrearTabla-tags.png)

En Database viendo a la tabla de "**tags**" ya podemos "**Añadir BREAD a esta tabla**".
Agregar un icono "**voyager-tag**", por ahorita es todo lo que vamos a modificar aquí, ahora dar click en el botón de "**Enviar**".

Ahora en side menu en "**Tags**", crear unas etiquetas, por ejemplo: 
- Programación
- Diseño

### Crear Tabla Intermedia
Ahora queremos relacionar los productos con las etiquetas.
Primero vamos a **crear una tabla intermedia**, por convención se suele nombrar a estas tablas intermedias con los dos nombres de los modelos en singular y separados por un guion, para nuestro caso seria "**product_tag**", **No** generar modelo, y agregamos unas columnas.

- "**product_id**", donde vamos a almacenar el id del producto
- "**tag_id**", donde vamos a almacenar el id de las etiquetas.

Añadir también el "**Timestamps**", y dar click en el botón de "**Crear tabla**".

![Widgets](/Voyager/creartabla-product_tag.png)

### Crear la relación
Dirigirnos al BREAD de products y dar click en el botón de "**Editar**", irnos a la parte final para agregar una nueva relación dar click en el botón de "**Crear una relación**", y configurar la relación de la siguiente manera.

![Widgets](/Voyager/relacion-product_tag.png)

La opción de "**Permitir Tagging**", nos sirve para poder agregar tags "*on the fly*" cuando estamos editando un producto, esto es muy funcional para evitar que el usuario tenga que ir primero a la tabla de tags y agregar primero un tag nuevo para después poder usarlo cuando estemos creando o editando un producto.



