---
date: 2023-03-04T17:22:55-08:00
title: "Extraer la clase"
menuTitle: "18. Extraer Clase"
draft: false
weight: 40
---

Ahora que comprendemos la lógica básica para inicial-izar una instancia de PDO y ejecutar una consulta preparada, aclaremos un poco las cosas extrayendo una clase de base de datos dedicada, vamos a extraer la funcionalidad del código a una clase de guardarla en su propio archivo **Database.php**.

## Refactor 1
Podemos empezar a refactor el código que llevamos, empezando por crear una clase que se llame `Database`, simplemente movemos por ahorita todo el código dentro de un método que llamemos `query()`, nos quedaría algo así:
```php
<?php 

require 'functions.php';
// require 'router.php';


// Conectarnos a la base de datos MySQL $dsn (data source name) y ejecutar un Query
class Database {
    public function query(){
        $host = '127.0.0.1';
        $dbname = 'phplaracast';
        $user = 'root';
        $pass = '';
        try
        {
            $DB = new PDO("mysql:host=$host;dbname=$dbname", $user, $pass); 
        }
        catch(PDOException $e)
        {  
            echo $e->getMessage();  
        }

        $statement = $DB->prepare("select * from posts");
        $statement->execute();
        return $statement->fetchAll(PDO::FETCH_ASSOC);
    }
}

$db = new Database;
$posts = $db->query();


// Desplegar los posts
foreach ($posts as $post){
    echo "<li>" . $post['title'] . "</li>";
}
```
Vemos que todo funciona correctamente!

## Refactor 2
Pasar el `$query` como parámetro al método.
```php
class Database {
    public function query($query){
        $host = '127.0.0.1';
        $dbname = 'phplaracast';
        $user = 'root';
        $pass = '';
        try
        {
            $DB = new PDO("mysql:host=$host;dbname=$dbname", $user, $pass); 
        }
        catch(PDOException $e)
        {  
            echo $e->getMessage();  
        }

        $statement = $DB->prepare($query);
        $statement->execute();
        return $statement->fetchAll(PDO::FETCH_ASSOC);
    }
}
$db = new Database;
$posts = $db->query("select * from posts");
```

## Refactor 3
Crear la instancia de conexión PDO solo una vez, podemos colocar el código de conexión al método especial de php llamado `__construct()` así:
```php
<?php 

require 'functions.php';
// require 'router.php';


// Conectarnos a la base de datos MySQL $dsn (data source name) y ejecutar un Query
class Database {

    public $connection='';

    public function __construct()
    {
        $host = '127.0.0.1';
        $dbname = 'phplaracast';
        $user = 'root';
        $pass = '';
        try
        {
            $this->connection = new PDO("mysql:host=$host;dbname=$dbname", $user, $pass);
        }
        catch(PDOException $e)
        {  
            echo $e->getMessage();  
        }    
    }

    public function query($query){

        $statement = $this->connection->prepare($query);
        $statement->execute();
        return $statement->fetchAll(PDO::FETCH_ASSOC);
    }
}

$db = new Database;
$posts = $db->query("select * from posts");


// Desplegar los posts
foreach ($posts as $post){
    echo "<li>" . $post['title'] . "</li>";
}
```

## Refactor 4
Para poder escoger entre fetch t fetchAll podemos regresar solo el return $statement; en el método query() y mandar llamar el tipo de fetch que necesitemos después:
```php
public function query($query){

	$statement = $this->connection->prepare($query);
	$statement->execute();
	return $statement;
}
```
Por ejemplo para mandar llamar solo un registro:
```php
$db = new Database;
$post = $db->query("select * from posts where id = 1")->fetch(PDO::FETCH_ASSOC);
dd($post['title']);
```
Lo cual nos despliega
`string(13) "Titulo Blog 1"`

Para mandar llamar todos los posts
```php
$db = new Database;
$posts = $db->query("select * from posts")->fetchAll(PDO::FETCH_ASSOC);
dd($posts);
```
Lo cual nos despliega
```bash
array(5) {
  [0]=>
  array(2) {
    ["id"]=>
    int(1)
    ["title"]=>
    string(13) "Titulo Blog 1"
  }
  [1]=>
  array(2) {
    ["id"]=>
    int(2)
    ["title"]=>
    string(13) "Titulo Blog 2"
  }
  [2]=>
  array(2) {
    ["id"]=>
    int(3)
    ["title"]=>
    string(13) "Titulo Blog 3"
  }
  [3]=>
  array(2) {
    ["id"]=>
    int(4)
    ["title"]=>
    string(13) "Titulo Blog 4"
  }
  [4]=>
  array(2) {
    ["id"]=>
    int(5)
    ["title"]=>
    string(8) "Titulo 5"
  }
}
```
Listo!	

Por ultimo mover toda la clase a su propio archivo `Database.php`, nota el primer carácter es 'D', esto es una convención cuando solo tenemos uno sola clase almacenada en el archivo, entonces queda:
Database.php
```php
<?php 

// Conectarnos a la base de datos MySQL $dsn (data source name) y ejecutar un Query
class Database {

    public $connection='';

    public function __construct()
    {
        $host = '127.0.0.1';
        $dbname = 'phplaracast';
        $user = 'root';
        $pass = '';
        try
        {
            $this->connection = new PDO("mysql:host=$host;dbname=$dbname", $user, $pass);
        }
        catch(PDOException $e)
        {  
            echo $e->getMessage();  
        }    
    }

    public function query($query){

        $statement = $this->connection->prepare($query);
        $statement->execute();
        return $statement;
    }
}
```
En index.php
```php
<?php 

require 'functions.php';
require 'Database.php';
// require 'router.php';

$db = new Database;
$posts = $db->query("select * from posts")->fetchAll(PDO::FETCH_ASSOC);
dd($posts);

// Desplegar los posts
foreach ($posts as $post){
    echo "<li>" . $post['title'] . "</li>";
}
```
Listo!