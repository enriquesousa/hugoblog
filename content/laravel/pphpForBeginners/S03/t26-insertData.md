---
date: 2023-03-13T08:54:15-07:00
title: "Insertar Datos a DB y Protegernos de Datos con Malicia"
menuTitle: "26. Insert Data"
draft: false
weight: 40
---

En esta lección, finalmente conservaremos una nueva nota en la base de datos. Pero, al hacerlo, se le presentará un nuevo problema de seguridad que requiere que siempre evitemos la entrada proporcionada por el usuario.

## Que aprenderemos
- Insert Queries
- htmlspecialchars()

## Insertar datos a DB
Recordemos los datos que llegan en la variable super global `$_POST`
Salida de `dd($_POST)`:
```php
array(1) {
  ["body"]=>
  string(6) "prueba"
}
```
Para insertar datos a la base de datos, en `controllers/note-create.php`:
```php
<?php 

$config = require('config.php');
$db = new Database($config['database']);

$heading = 'Crear Nota';

if($_SERVER['REQUEST_METHOD'] === 'POST'){
    $db->query('INSERT INTO notes (body, user_id) VALUES(:body, :user_id)', [
        'body' => $_POST['body'],
        'user_id' => 1
    ]);
}

require 'views/note-create.view.php';
```

Para protegernos de datos con malicia como podría ser una nota con código script java, por ejemplo:
`<h1 style="font-size: 100px">Ah ja</h1><script>alert('Hola desde JS')</script>` podemos utilizar una función de de php `htmlspecialchars()` en nuestra vista `views/notes.view.php`:
```php
<?php foreach ($notes as $note) : ?>
	<li>
		<a href="/note?id=<?=$note['id'] ?>" class="text-blue-500 hover:underline">
			<?= htmlspecialchars($note['body']) ?>
		</a>
	</li>
<?php endforeach; ?>
```
Y hacer lo mismo en las demás partes donde desplegamos.
En `views/note.view.php`
```php
<main>
	<div class="mx-auto max-w-7xl py-6 sm:px-6 lg:px-8">
		<p class="mb-6">
			<a href="/notes" class="text-blue-500 underline">Regresar...</a>
		</p>
		<?= htmlspecialchars($note['body']) ?>
	</div>
</main>
```
Todavía falta mucho por hacer, que pasa si damos salvar nota con los datos vacíos pro ejemplo, vemos que no estamos protegidos contra eso, para esto necesitamos otra capa de protección (form validation) que lo veremos la próxima lección.




