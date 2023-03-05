---
date: 2023-03-04T19:14:26-08:00
title: "Entornos y flexibilidad de configuración"
menuTitle: "18. Entornos"
draft: false
weight: 45
---

Tenemos un problema evidente con nuestra clase de base de datos en este momento. Los valores de conexión han sido codificados de forma rígida. Entonces, ¿qué sucede cuando llevamos el proyecto a producción, donde el host y el puerto son completamente diferentes?

Vamos a pasar la constante de PDO `[PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC]` dentro de la clase, y vamos a utilizar una nueva función de php `http_build_query($data)`, que es lo que hace esta función?

## Refactor 1 - Database.php
Nuestro primer intento podría verse algo así:
```php
<?php 

// Conectarnos a la base de datos MySQL $dsn (data source name) y ejecutar un Query
class Database {
    public $connection='';
    public function __construct(){
        $config = [
            'host' => '127.0.0.1',
            'port' => 3306,
            'dbname' => 'phplaracast',
            'charset' => 'utf8mb4',
        ];

        $dsn = "mysql:host={$config['host']}, port={$config['port']}, dbname={$config['dbname']}, charset={$config['charset']} ";
        $this->connection = new PDO($dsn, 'root', '', [PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC]);          
    }

    public function query($query){
        $statement = $this->connection->prepare($query);
        $statement->execute();
        return $statement;
    }
}
```

## http_build_query()
Lo que hace esta función es construir el query string por ejemplo, `example.com?host=127.0.0.1&port=3306&dbname=phplaracast` etc.. donde el query string es `host=127.0.0.1&port=3306&dbname=phplaracast`, para poder usarlo dentro de nuestro código tenemos que quitar los `&`, entonces podemos cambiar el `&` por `;` como lo requiere nuestro $dsn de la siguiente manera:
```php
$config = [
	'host' => '127.0.0.1',
	'port' => 3306,
	'dbname' => 'phplaracast',
	'charset' => 'utf8mb4',
];

dd(http_build_query($config, '', ';'));
```
Nos da la salida
`string(59) "host=127.0.0.1;port=3306;dbname=phplaracast;charset=utf8mb4"`
Podemos ver como ya empezamos a construir nuestro $dsn.
Para incluir el `mysql:` que necesitamos para el $dsn podemos concatenar el string así:
`dd('mysql:' . http_build_query($config, '', ';'));`
Lo cual nos da:
`string(65) "mysql:host=127.0.0.1;port=3306;dbname=phplaracast;charset=utf8mb4"`
Y es precisamente el $dsn que estamos buscando.

## Refactor 2 - Database.php
Entonces ya podemos simplificar nuestro $dsn así:
```php
class Database {
    public $connection='';
    public function __construct(){
        $config = [
            'host' => '127.0.0.1',
            'port' => 3306,
            'dbname' => 'phplaracast',
            'charset' => 'utf8mb4',
        ];

        $dsn = 'mysql:' . http_build_query($config, '', ';');
        $this->connection = new PDO($dsn, 'root', '', [PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC]);
    }

    public function query($query){

        $statement = $this->connection->prepare($query);
        $statement->execute();
        return $statement;
    }
}
```
Todo funciona bien hasta aquí!

## Refactor 3
Ahora tenemos el problema de que las variables de configuración están grabadas en código dentro de la clase. Como un primer intento de sacar estas variables de la clase, podemos subirlas un nivel quedando así: 
En **Database.php**
```php
class Database {
    public $connection='';

    public function __construct($config){

        $dsn = 'mysql:' . http_build_query($config, '', ';');
        $this->connection = new PDO($dsn, 'root', '', [PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC]);
        
    }

    public function query($query){

        $statement = $this->connection->prepare($query);
        $statement->execute();
        return $statement;
    }
}
```
Y le mandamos los los parámetros de configuración desde **index.php** así:
```php
$config = [
    'host' => '127.0.0.1',
    'port' => 3306,
    'dbname' => 'phplaracast',
    'charset' => 'utf8mb4',
];

$db = new Database($config);
```
Y todo sigue funcionando igual, pero aun siguen las variables de configuración grabadas en el código, en nuestro siguiente intento podemos subirlas otro nivel y sacarlas a un archivo de configuración donde vivirán todas nuestras `variables de entorno`, llamemos a ese archivo `config.php`:
```php
<?php 

return [
    'host' => '127.0.0.1',
    'port' => 3306,
    'dbname' => 'phplaracast',
    'charset' => 'utf8mb4',
];
```
Ahora en vez de declarar una variable, vamos hacer un `return`.
Y en `index.php` podemos requerir el archivo así:
```php
$config = require('config.php');
$db = new Database($config);
```
Y todo sigue funcionando!
Aprendimos que la función `return` no solo se usa en funciones, si no que también la podemos usar en archivos como este, lo que le estamos diciendo a php con la instrucción `$config = require('config.php');` es `$config` va a ser igual a lo que regrese el requerir al archivo `config.php`.
Si analizamos la clase en `Database.php` todavía tenemos pendiente refactor tanto el `user` como su `password`.

## Refactor 4
```php
public function __construct($config, $username = 'root', $password = ''){
        $dsn = 'mysql:' . http_build_query($config, '', ';');
        $this->connection = new PDO($dsn, $username, $password, [PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC]);
    }
```

## Refactor 5
Finalmente podemos agrupar las variables de `config.php` en una llave que le podemos llamar `database`:
```php
<?php 

return [

    'database' => [
        'host' => '127.0.0.1',
        'port' => 3306,
        'dbname' => 'phplaracast',
        'charset' => 'utf8mb4',
    ],

];
```
Y mandarla llamar desde `index.php` así:
```php
$config = require('config.php');
$db = new Database($config['database']);
```
Listo!
Todo sigue funcionando correctamente!
Pero ahora nuestro código a quedada sumamente mejorado.


