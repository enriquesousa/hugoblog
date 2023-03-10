---
date: 2023-03-10T14:34:37-08:00
title: "PDO fetch() y fetchall() Override"
menuTitle: "24. PDO fetch Override"
draft: false
weight: 30
---

Antes de continuar con la creación de un formulario para conservar las notas nuevas en la base de datos, tomemos diez minutos para refactorizar nuestro código actual y discutir el cierre de las API que no son de su propiedad.

Vamos a `controllers/note.php` personalizar nuestro propio fetch() en 
```php
$note = $db->query('SELECT * FROM mis_notas.notes WHERE id = :id', [
                    'id' => $_GET['id']
                ])->fetch();
```
Para poder ahorrarnos el código 
```php
if (! $note){
    abort(Response::NOT_FOUND);
}
```
Le podríamos llamar findOrAbort().

Después de hacer los `tweak` correspondientes nos queda el código así:  
En `controllers/note.php`
```php
<?php 

$config = require('config.php');
$db = new Database($config['database']);

$heading = 'Nota';
$currentUserId = 1;

// dd($_GET['id']); // verificar que tenemos el id de la nota correcto
$note = $db->query('SELECT * FROM mis_notas.notes WHERE id = :id', [
                    'id' => $_GET['id']
                ])->findOrFail();

authorize($note['user_id'] === $currentUserId);

require "views/note.view.php";
```
En `Database.php`
```php
<?php 

// Conectarnos a la base de datos MySQL $dsn (data source name) y ejecutar un Query
class Database {
    
    public $connection='';
    public $statement;

    public function __construct($config, $username = 'root', $password = ''){
        $dsn = 'mysql:' . http_build_query($config, '', ';');
        $this->connection = new PDO($dsn, $username, $password, [PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC]);
    }

    public function query($query, $params = []){
        $this->statement = $this->connection->prepare($query);
        $this->statement->execute($params);
        return $this;
    }

    public function find(){
        return $this->statement->fetch();
    }

    public function findOrFail(){
        $result = $this->find();

        if (! $result) {
            abort(Response::NOT_FOUND);
        }
        return $result;
    }

}
```
En `functions.php`
```php
<?php 

// función Dump and Die
function dd($value) {
    echo "<pre>";
    var_dump($value);
    echo "</pre>"; 
    die(); 
}

// Me regresa true or false
function urlIs($value){
    return $_SERVER['REQUEST_URI'] === $value;
}

// check si el usuario esta autorizado
function authorize($condition, $status = Response::FORBIDDEN){
    if (! $condition) {
    abort($status); 
    }
}
```
Funciona pero aun tenemos un error al visitar:
`http://localhost:8888/notes`
nos falta resolver resolver la llamada a `fetchAll()` en `controllers/notes.php`, que les parece que en vez de llamarle `fetchAll()` le llamamos simplemente `get()`, no olvidar implementar el metodo en `Database.php`, entonce en `controllers/notes.php`:
```php
<?php 

$config = require('config.php');
$db = new Database($config['database']);

$heading = 'Mis Notas';

$notes = $db->query('SELECT id, body, user_id FROM mis_notas.notes WHERE user_id = 1')->get();

require "views/notes.view.php";
```
Y en `Database.php`:
```php
public function get(){
	return $this->statement->fetchAll();
}
```

`Nota:` Como ahora nosotros controlamos los métodos de fetchAll() y fetch() que antes controlaba solo la instancia de PDO, ahora nosotros controlamos estos métodos! usando nuestro propios.



