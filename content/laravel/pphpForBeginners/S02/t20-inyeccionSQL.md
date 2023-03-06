---
date: 2023-03-06T11:36:26-08:00
title: "Explicación de las vulnerabilidades de inyección SQL"
menuTitle: "20. Inyección SQL"
draft: false
weight: 50
---

En este episodio, revisaremos algunos ejemplos de inyección SQL y discutiremos por qué es tan importante que siempre asumas que, en la web, una persona es culpable hasta que se demuestre su inocencia.

Pasar parámetros por el query string, por ejemplo
`http://localhost:8888/?id=1`

Para acceder a los parámetros del query string usamos
`$_GET`
Podemos usarlo así:
```php
$config = require('config.php');
$db = new Database($config['database']);
dd($_GET);
```
Entonces una de las formas que podemos desplegar el post id que nos pasen por el query string podría verse así:
```php
$config = require('config.php');
$db = new Database($config['database']);

$id = $_GET['id'];

$posts = $db->query("select * from posts where id = {$id}")->fetch();
dd($posts);
```
Y si funciona, pero, el problema que tenemos es que introducimos una invulnerabilidad muy grabe, ya que estamos permitiendo al usuario entrar código SQL a nuestro código.
Imagina que un usuario mal intencionado escribe un query string como el siguiente:
`localhost:8888/?id=1; drop table users;` 
Como puedes ver, esto seria desastroso para nuestra aplicación.

Una solución podría ser, pasar el la variable `$id` como un parámetro al método  `$db->query()`  en `index.php` así:
```php
$config = require('config.php');
$db = new Database($config['database']);

$id = $_GET['id'];
$query = "select * from posts where id = ?";

// bind (unir) $id
$posts = $db->query($query, [$id])->fetch();
dd($posts);
```
Y ahora preparamos que pueda recibir el parámetro el método en `Database.php` así:
```php
public function query($query, $params = []){
	$statement = $this->connection->prepare($query);
	$statement->execute($params);
	return $statement;
}
```
Ya funciona, aunque en el query string metamos datos maliciosos así:
`http://localhost:8888/?id=1;%20drop%20table%20users;`
El sistema nos regresa correctamente el post 1.

Una mejora adicional es constringir el parámetro y darle el valor de una llave `:id` (key), en `index.php` así:
```php
$config = require('config.php');
$db = new Database($config['database']);

$id = $_GET['id'];
$query = "select * from posts where id = :id";

// bind (unir) $id
$posts = $db->query($query, [':id' => $id])->fetch();
dd($posts);
```
Listo!
Funciona, tambien podriamos si queremos omitir los `:` en:
```php
$posts = $db->query($query, ['id' => $id])->fetch();
dd($posts);
```
Listo!
Funciona igual.

**Nota!**
Lo mas importante que aprendimos en este tema es que **Nunca** de los **Nunca** colocar código en linea `in-line` de parámetros SQL a nuestro `$query`.



