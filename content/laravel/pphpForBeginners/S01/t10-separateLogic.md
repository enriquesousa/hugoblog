---
date: 2023-03-01T09:17:26-08:00
title: "Separemos la Lógica de la plantilla"
menuTitle: "10. Separemos Lógica"
draft: false
weight: 50
---

Antes de continuar con el control técnico de este capítulo, dediquemos un poco de tiempo a discutir la organización del código y por qué podríamos querer separar nuestra lógica PHP de la vista o HTML.

## Mover código
Mover código php a parte superior, como un primer paso podemos mover el código php y a la parte superior del archivo html así:

{{< details "Código PHP" >}}
```php
<?php
        
    $books = [
        [
            'name' => 'Do Androids Dream of Electric Sheep?',
            'author' => 'Philip K. Dick',
            'purchaseUrl' => 'http://example1.com',
            'publicationDate' => 1968,
        ],

        [
            'name' => 'Four Past Midnight',
            'author' => 'Stephen King',
            'purchaseUrl' => 'http://example2.com',
            'publicationDate' => 1990,
        ],

        [
            'name' => 'Project Hail Mary',
            'author' => 'Andy Weir',
            'purchaseUrl' => 'http://example3.com',
            'publicationDate' => 2021,
        ],

        [
            'name' => 'The Martian',
            'author' => 'Andy Weir',
            'purchaseUrl' => 'http://example4.com',
            'publicationDate' => 2011,
        ],
    ];

    // Mandar llamar la **built-in** function de PHP mejor! 
    $filteredBooks = array_filter($books, function ($book){
        return $book['author'] === 'Andy Weir';
    });    
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            display: grid;
            place-items: center;
            /* height: 100vh; */
            margin: 0;
            font-family: sans-serif;
        }
    </style>
</head>
<body>
    <h1>Libros Recomendados</h1>
    <ul>
        <?php foreach ($filteredBooks as $book) : ?>
            <li>
                <a href="<?= $book['purchaseUrl'] ?>">
                    <?= $book['name'] ?> (<?= $book['publicationDate']?>) - By <?= $book['author'] ?>
                </a>
            </li>
        <?php endforeach; ?>
    </ul>
</body>
</html> 
```
{{< /details >}}


## En index.view.php
Como un segundo paso podemos separar el código html en su propio archivo index.view.php y para llamarlo podemos usar required o include, nota como podemos eliminar `?>` del archivo que contiene solo código php.

{{< details "Código PHP" >}}
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            display: grid;
            place-items: center;
            /* height: 100vh; */
            margin: 0;
            font-family: sans-serif;
        }
    </style>
</head>
<body>
    <h1>Libros Recomendados</h1>
    <ul>
        <?php foreach ($filteredBooks as $book) : ?>
            <li>
                <a href="<?= $book['purchaseUrl'] ?>">
                    <?= $book['name'] ?> (<?= $book['publicationDate']?>) - By <?= $book['author'] ?>
                </a>
            </li>
        <?php endforeach; ?>
    </ul>
</body>
</html> 
```
{{< /details >}}

## En index.php
{{< details "Código PHP" >}}
```php
<?php
        
$books = [
    [
        'name' => 'Do Androids Dream of Electric Sheep?',
        'author' => 'Philip K. Dick',
        'purchaseUrl' => 'http://example1.com',
        'publicationDate' => 1968,
    ],

    [
        'name' => 'Four Past Midnight',
        'author' => 'Stephen King',
        'purchaseUrl' => 'http://example2.com',
        'publicationDate' => 1990,
    ],

    [
        'name' => 'Project Hail Mary',
        'author' => 'Andy Weir',
        'purchaseUrl' => 'http://example3.com',
        'publicationDate' => 2021,
    ],

    [
        'name' => 'The Martian',
        'author' => 'Andy Weir',
        'purchaseUrl' => 'http://example4.com',
        'publicationDate' => 2011,
    ],
];

// Mandar llamar la **built-in** function de PHP mejor! 
$filteredBooks = array_filter($books, function ($book){
    return $book['author'] === 'Andy Weir';
});    

require "index.view.php";
```
{{< /details >}}

**Listo!** {{< line_break >}}

Con esto efectivamente separamos la lógica de php con la vista que solo se dedica al render de la pagina que queremos que el usuario pueda ver!
**Nota:** Como las variables como `$filteredBooks` pueden ser vista por el código en la vista!, esto es gracias al la instrucción de php `require "index.view.php";` que usamos para mandar llamar nuestra vista.

