---
date: 2023-03-07T12:15:34-08:00
title: "Database Tables and Indexes"
menuTitle: "21. Crear Tablas"
draft: false
weight: 15
---

Has aprendido lo suficiente de los fundamentos en este momento para comenzar a trabajar en tu primer mini proyecto. ¡Hagamos una aplicación de notas! Comenzaremos con la estructura inicial de la base de datos, que nos dará la oportunidad de revisar los índices de MySQL.

## Que aprenderemos
- Table Relationships
- Indexes


Vamos a crear nueva base de datos:
- mis_notas
Con las Tablas:
- notes
	- id INT, Primary Key, Auto Increment
	- body Text not null
	- user_id Foreign_Key users(id)
- users
	- id INT, Primary Key, Auto Increment
	- name Varchar(255), not null
	- email Varchar(255), not null, (unique)

Todo lo vamos hacer a nivel base de datos, con el Database Manager `DBeaver`.

## Paso 1 - Conectarnos a MySQL Local Host
![Widgets](/phpforbegginers/db_config.png)
Test Connection y confirmar que se conecto correctamente!

## Paso 2 - Crear Nueva Base de Datos
![Widgets](/phpforbegginers/crear_new_db.png)

## Paso 3 - Crear Tabla `notes`
![Widgets](/phpforbegginers/crear_tabla_notes.png)

## Paso 4 - Crear Campos id (Columna)
![Widgets](/phpforbegginers/agregar_columna.png)

### id
![Widgets](/phpforbegginers/atributos_id_config.png)

### Asignar Primary Key a `id`
![Widgets](/phpforbegginers/Primary_key_id.mp4)

## Paso 5 - Crear Tabla `users`
Con el mismo procedimiento anterior creamos los campos `id` y `name`.

### Hacer el campo de email `unique`
Tenemos que generar este código:
`CREATE UNIQUE INDEX users_email_IDX USING BTREE ON mis_notas.users (email);`
Para generar este código, estando en las propiedades de la tabla users, dar click derecho en el campo `email` y escoger la opción `new index from selection` y seleccionar la opción (check box) de unique. 
![Widgets](/phpforbegginers/email_unique.png)

Y dar click a Salvar!
![Widgets](/phpforbegginers/Save_Unique.png)
Con esto se genera el código necesario para crear el `unique field`

Por ultimo click en `Persist`. 
![Widgets](/phpforbegginers/persist.png)

### Así quedan los campos de la tabla `users`
![Widgets](/phpforbegginers/campos_users.png)

## Paso 6 - Llave foránea `user_id` en tabla `notes`
Para la llave foránea (foreign key) `user_id` para poder relacionar que un usuario creo esta nota, agregamos el campo `user_id` a la tabla `notes` 

Para Entrar a la pantalla de `foreign key` en `DBeaver`
![Widgets](/phpforbegginers/crear_new_foreign_key.png)

Y asignar en la pantalla el `foreign key` así:
![Widgets](/phpforbegginers/ventana_foreignkey.png)

`ALTER TABLE mis_notas.notes ADD CONSTRAINT notes_FK FOREIGN KEY (user_id) REFERENCES mis_notas.users(id) ON DELETE CASCADE;`

Usamos la propiedad `CASCADE` de la base de datos MySQL para poder borrar todas las notas relacionadas con el usuario, cuando decidimos borrar al un usuario de nuestra base de datos, tabla `users`.

### Así quedan los campos de la tabla `notes`
![Widgets](/phpforbegginers/campos_final_notes.png)

### Todo el `DDl` queda así 
DDL (El lenguaje de definición de datos)
```mysql
CREATE TABLE `notes` (
  `id` int NOT NULL AUTO_INCREMENT,
  `body` text NOT NULL,
  `user_id` int NOT NULL,
  PRIMARY KEY (`id`),
  KEY `notes_FK` (`user_id`),
  CONSTRAINT `notes_FK` FOREIGN KEY (`user_id`) REFERENCES `users` (`id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci COMMENT='Tabla para las notas';
```
