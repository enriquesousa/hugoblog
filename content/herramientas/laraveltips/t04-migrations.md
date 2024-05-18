---
title: "Migrations"
date: 2024-05-18T09:58:22-07:00
draft: true
---

##  Migrations - What is Migrations and how to use them.
*Crear la una migración para crear la tabla `blogs`.* 
<br>


## Crear tabla
`php artisan make:migration create_blogs_table` 	
<br>

## Table blogs
`En database/migrations/2024_05_18_163224_create_blogs_table.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
Schema::create('blogs', function (Blueprint $table) {
	$table->id();

	$table->string('title');
	$table->text('body');
	$table->boolean('status');

	$table->timestamps();
});
```
</details>
<br>

## Migrar
`php artisan migrate`
Si hacemos un cambio a la tabla, por ejemplo agregar un nuevo campo, podemos volver a migrar pero ahora usamos.
`php artisan migrate:fresh`
<br>


* * *
Listo.

