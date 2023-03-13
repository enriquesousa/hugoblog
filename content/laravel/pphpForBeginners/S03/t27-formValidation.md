---
date: 2023-03-13T12:56:16-07:00
title: "Validación de la Forma"
menuTitle: "27. Form Validation"
draft: false
weight: 45
---

En esta lección, revisaremos dos capas de validación de formularios: navegador y servidor. Podemos utilizar la validación para garantizar y confirmar que el usuario envía sus datos en el formato exacto que requerimos.

## Que aprenderemos
- Browser Validation
- Server-side Validation
- strlen()

## Validación cliente
Validación de parte del front-end (Client Side Validation).
Agregar el atributo `required` a `views/note-create.view.php`
```php
<!-- Body -->
<div>
	<label for="body" class="block text-sm font-medium leading-6 text-gray-900">Nota</label>
	<div class="mt-2">
		<textarea 
			id="body" 
			name="body" 
			rows="3" 
			class="mt-1 block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:py-1.5 sm:text-sm sm:leading-6" 
			placeholder="Aquí escribe tu nota! ..."
			required
		></textarea>
	</div>
</div>
```
Aun así, seguimos teniendo una vulnerabilidad, si el usuario manda la información por le url, por ejemplo:
`curl -X POST http://localhost:8888/notes/create -d 'body='`
Con esto el usuario sigue agregando una nueva nota vacía a nuestra base de datos.
Este tipo de validación que acabamos de escribir en nuestra etiqueta `<textarea>` se le conoce como validación del lado del cliente, y es buena, pero no suficiente, el siguiente paso es agregar una capa de seguridad a nuestro servidor.

## Validación Server
Validación de parte del servidor.
En nuestro controlador `controllers/note-create.php` antes de realizar el `query` podemos verificar que el `body` cumpla con nuestra regla.
```php
if($_SERVER['REQUEST_METHOD'] === 'POST'){
    
    $errors = [];
    if (strlen($_POST['body']) === 0) {
        $errors['body'] = 'Se requiere contenido';
    }

    if (empty($errors)) {
        $db->query('INSERT INTO notes (body, user_id) VALUES(:body, :user_id)', [
            'body' => $_POST['body'],
            'user_id' => 1
        ]);
    }
}
```
Y desplegamos el error al usuario en la vista `views/note-create.view.php`
```php
<div class="mt-2">
	<textarea 
		id="body" 
		name="body" 
		rows="3" 
		class="mt-1 block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:py-1.5 sm:text-sm sm:leading-6" 
		placeholder="Aquí escribe tu nota! ..."
	></textarea>

	<?php if (isset($errors['body'])) : ?>
		<p class="text-red-500 text-xs mt-2"><?= $errors['body'] ?></p>
	<?php endif; ?>
</div>
```
Ahora para controlar la cantidad de texto que los usuarios puedan ingresas en la nota!
```php
if($_SERVER['REQUEST_METHOD'] === 'POST'){
    
    $errors = [];
    if (strlen($_POST['body']) === 0) {
        $errors['body'] = 'Se requiere contenido.';
    }
    if (strlen($_POST['body']) > 1000) {
        $errors['body'] = 'El texto no puede exceder 1000 caracteres.';
    }

    if (empty($errors)) {
        
        $db->query('INSERT INTO notes (body, user_id) VALUES(:body, :user_id)', [
            'body' => $_POST['body'],
            'user_id' => 1
        ]);

    }
}
```
Siguiente detalle que podemos mejorar es, consistir los datos, la forma larga es usando lógica ternaria en `views/note-create.view.php`:
```php
<textarea 
	id="body" 
	name="body" 
	rows="3" 
	class="mt-1 block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:py-1.5 sm:text-sm sm:leading-6" 
	placeholder="Aquí escribe tu nota! ..."
><?= isset($_POST['body']) ? $_POST['body'] : '' ?></textarea>
```
El equivalente mas moderno ya con las ventajas de php 8.1, queda así:
```php
<textarea 
	id="body" 
	name="body" 
	rows="3" 
	class="mt-1 block w-full rounded-md border-0 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:py-1.5 sm:text-sm sm:leading-6" 
	placeholder="Aquí escribe tu nota! ..."
><?= $_POST['body'] ?? '' ?></textarea>
```
Queda mucho mas compacto!
Con esta mejora, aunque mostremos el error de `'El texto no puede exceder 1000 caracteres.'` seguimos conservando los datos en el area de texto para poder seguir editando.

En la siguiente lección vamos a extraer todo este código de validación en su propia clase.




