---
date: 2023-03-14T18:57:52-07:00
title: "¿Manejar múltiples métodos de solicitud desde una acción de controlador?"
menuTitle: "32. Delete Note"
draft: false
weight: 35
---

En los próximos tres episodios, revisaremos una serie de refactorizaciones que son un poco más avanzadas. Pero primero, necesitamos encontrar una situación que requiera los refactores. Usaremos el ejemplo de una acción de controlador desordenada que puede responder a múltiples tipos de solicitudes.

## Que aprenderemos
- Request Methods
- Delete Forms

## Agregar botón Eliminar Nota
En `views/notes/show.view.php`:
```php
<!-- Contenido -->
<main>
    <div class="mx-auto max-w-7xl py-6 sm:px-6 lg:px-8">

        <p class="mb-6">
            <a href="/notes" class="text-blue-500 underline">Regresar...</a>
        </p>

        <p>
            <?= htmlspecialchars($note['body']) ?>
        </p>

        <form class="mt-6" method="POST">
            <input type="hidden" name="id" value="<?= $note['id'] ?>">
            <button class="text-sm text-red-500">Eliminar</button>
        </form>

    </div>
</main>
```
Usamos una forma para `submit data` a nuestro controlador `controllers/notes/show.php` y en el controlador verificamos usuario y corremos query para eliminar la nota con `id = $note['id']`:
```php
<?php 
use Core\Database;

$config = require base_path('config.php');
$db = new Database($config['database']);

$currentUserId = 1; // current user id (hard code)

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    // dd($_POST);

    // Check que sea el usuario autenticado
    $note = $db->query('SELECT * FROM mis_notas.notes WHERE id = :id', [
            'id' => $_GET['id']
        ])->findOrFail();
    authorize($note['user_id'] === $currentUserId);

    // form wa submitted. delete the current note
    $db->query('DELETE FROM notes WHERE id = :id', [
        'id' => $_GET['id'] 
    ]);

    // Redirect a la pagina de show notes
    header('location: /notes');
    exit();

} else {

    // dd($_GET['id']); // verificar que tenemos el id de la nota correcto
    $note = $db->query('SELECT * FROM mis_notas.notes WHERE id = :id', [
                        'id' => $_GET['id']
                    ])->findOrFail();
    authorize($note['user_id'] === $currentUserId);

    view("notes/show.view.php", [
        'heading' => 'Nota',
        'note' => $note,
    ]);

}
```
Listo!
