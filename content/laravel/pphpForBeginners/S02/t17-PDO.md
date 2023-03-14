---
date: 2023-03-03T18:56:12-08:00
title: "PDO Primeros pasos"
menuTitle: "17. PDO"
draft: false
weight: 35
---

El siguiente paso en nuestro viaje es descubrir cómo conectarnos a MySQL desde PHP y ejecutar una consulta SELECT simple. Por supuesto, utilizaremos PHP Data Objects, o PDO, para orquestar esta tarea de forma segura.

## Que aprenderemos
- Select Queries
- DSNs
- PDO


## PDO (PHP Data Objects)
El siguiente paso en nuestro viaje es descubrir cómo conectarnos a MySQL desde PHP y ejecutar una consulta SELECT simple. Por supuesto, utilizaremos PHP Data Objects, o PDO, para orquestar esta tarea de forma segura.

La extensión PHP Data Objects (PDO) define una interfaz liviana y consistente para acceder a bases de datos en PHP. Cada controlador de base de datos que implementa la interfaz PDO puede exponer características específicas de la base de datos como funciones de extensión regulares. Tenga en cuenta que no puede realizar ninguna función de base de datos usando la extensión PDO por sí misma; debe usar un controlador PDO específico de la base de datos para acceder a un servidor de base de datos.

PDO proporciona una capa de abstracción de acceso a datos, lo que significa que, independientemente de la base de datos que esté utilizando, utiliza las mismas funciones para emitir consultas y obtener datos. PDO no proporciona una abstracción de base de datos; no reescribe SQL ni emula funciones faltantes. Debe usar una capa de abstracción completa si necesita esa función.

PDO se envía con PHP.

## Clases
Como funcionan las clases.
Las Clases no son más que una serie de variables y funciones que describen y actúan sobre algo. Por ejemplo, vamos a crear la clase Person , la cual tendrá diversas variables, $name , $age y habrá una serie de funciones que actuarán sobre la clase Person como breathe(), etc.. 
Para generar una instancia de esa clase usamos `$person = new Person();` y después podemos asignarle sus propiedades así:
```php
$person->name = 'John Doe';
$person->age = 25;
``` 
El código con todo y clase y llamando su método puede quedar así:
```php
Clases
class Person{
    public $name;
    public $age;

    public function breathe(){
        echo $this->name .  " is breathing";
    }
}
$person = new Person();
$person->name = 'John Doe';
$person->age = 25;
dd($person->name);
dd($person->age);
$person->breathe();
```
Con esto nos da una muy buena idea como funcionan las clases.

Ahora para poder conectarnos a la base de datos que tenemos con nuestro servidor de MySQL en localhost(127.0.0.1), vamos a usar una clase que ya viene con PHP llamada `PDO`. 
Para usarla, `index.php` lo modificamos así:
```php
<?php 

require 'functions.php';
// require 'router.php';


// Conectarnos a la base de datos MySQL $dsn (data source name)
// $dsn = "mysql:host=localhost;port=3306;dbname=myapp2;charset=utf8";
// $pdo = new PDO($dsn, 'root');
// nota: localhost me funciono con 127.0.0.1 y no con 'localhost' 
// find pdo in php.
// Source: https://stackoverflow.com/a/13165628 .
// Obviously, replace these with your own values
$host = '127.0.0.1';
$dbname = 'myapp2';
$user = 'enrique';
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

$posts = $statement->fetchAll(PDO::FETCH_ASSOC);
// dd($posts);

foreach ($posts as $post){
    echo "<li>" . $post['title'] . "</li>";
}
```

## Cambiar de DB
Fácilmente podemos cambiar a otra base de datos, primero asegurarnos de crear la nueva base da datos, por ejemplo 'phplaracast', después solo cambiar lo datos de $dbname, podemos cambiar si así lo deseamos al usuario de la base de datos en $user.
```php
$host = '127.0.0.1';
$dbname = 'phplaracast';
$user = 'root';
$pass = '';
```

## Buenas Practicas
$**dsn** - data source name
$**pdo** - php data objects











