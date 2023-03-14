---
date: 2023-03-13T20:38:57-07:00
title: "Convenciones de nomenclatura ingeniosas"
menuTitle: "29. Nomenclatura"
draft: false
weight: 20
---

Hagamos una breve pausa en el ejercicio de notas y, en su lugar, dirijamos nuestra atención a la organización general del código. Comenzaremos cambiando a una convención de nomenclatura común para los recursos.

## Que aprenderemos
- Resources
- Common Action Names

## Controladores y Vistas
### Ahorita tenemos tres archivos dedicados a notes:
- notes.php
- note.php
- note-create.php
### Y en las vistas:
- notes.view.php
- note.view.php
- note-create.view.php

## Convención en los controladores
index.php: Para mostrar todas las notas
show.php: Para mostrar solo una nota
create.php: Para crear una nueva nota

## Requerir acceder a los directorios
En `views/notes/index.view.php` podemos acceder a los requires así:
`<?php require(__DIR__ . '/../partials/head.php') ?>`
o así:
`<?php require('views/partials/head.php') ?>`

## Re acomodar la estructura de las carpetas
`Controladores`
- controllers/notes/create.php
- controllers/notes/index.php
- controllers/notes/show.php
`Vistas`
- views/notes/create.view.php
- views/notes/index.view.php
- views/notes/show.view.php

Cambiar los accesos a las carpetas en las vistas:
```php
<?php require('views/partials/head.php') ?>
<?php require('views/partials/nav.php') ?>
<?php require('views/partials/banner.php') ?>
<main>
	... Contenido ...
</main>
<?php require('views/partials/footer.php') ?>	
```

