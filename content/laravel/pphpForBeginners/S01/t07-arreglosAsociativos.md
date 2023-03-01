---
date: 2023-03-01T09:16:18-08:00
title: "Arreglos Asociativos (Associative Arrays)"
menuTitle: "07. Arreglos Asociativos"
draft: false
weight: 35
---

Sigamos con las matrices un poco más. En este episodio, aprenderá la sintaxis para acceder a elementos individuales dentro de una matriz. También aprenderá sobre matrices asociativas, que le permiten asociar una clave con cada valor.

Para poder acceder a elementos individuales dentro de un arreglo.
Por ejemplo para recuperar el primer libro del arreglo.
```php
<p>
    <?= $books[0] ?>
</p> 
```
Ahora si deseamos tener mas datos asociados a cada libro.
Podemos usar arreglos dentro de arreglos.
```php
<?php 
    $books = [
        [],
        []
    ];
?> 
```
Ahora vamos agregar una asociación key:value pair!
```php
<?php
    $books = [
        [
            'name' => 'Do Androids Dream of Electric Sheep?',
            'author' => 'Philip K. Dick',
            'purchaseUrl' => 'http://example1.com'
        ],
        [
            'name' => 'Four Past Midnight',
            'author' => 'Stephen King',
            'purchaseUrl' => 'http://example2.com'
        ],
        [
            'name' => 'Project Hail Mary',
            'author' => 'Andy Weir',
            'purchaseUrl' => 'http://example3.com'
        ]
    ];
?>

<ul>
    <?php foreach ($books as $book) :?>
        <li><?= $book['name']; ?></li>
    <?php endforeach; ?>
</ul> 
```
Ahora para mejorarlo un poco e incluir los links a los Urls
```php
<ul>
    <?php foreach ($books as $book) :?>
        <li>
            <a href="<?= $book['purchaseUrl'] ?>">
                <?= $book['name']; ?>
            </a>
        </li>
    <?php endforeach; ?>
</ul>
```

**Tarea:**
Amplíe la lista de libros del ejemplo de este episodio para incluir y mostrar también el año de lanzamiento inmediatamente después del título del libro.
```php
<body>

    <h1>Libros Recomendados</h1>

    <?php
        $books = [
                    [
                        'name' => 'Do Androids Dream of Electric Sheep?',
                        'author' => 'Philip K. Dick',
                        'purchaseUrl' => 'http://example1.com',
                        'Publication_date' => 1968,
                    ],

                    [
                        'name' => 'Four Past Midnight',
                        'author' => 'Stephen King',
                        'purchaseUrl' => 'http://example2.com',
                        'Publication_date' => 1990,
                    ],

                    [
                        'name' => 'Project Hail Mary',
                        'author' => 'Andy Weir',
                        'purchaseUrl' => 'http://example3.com',
                        'Publication_date' => 2021,
                    ]
                ];
    ?>

    <ul>
        <?php foreach ($books as $book) :?>
            <li>
                <a href="<?= $book['purchaseUrl'] ?>">
                    // <?= $book['name']." "."(".$book['Publication_date'].")"; ?>
                    <?= $book['name'] ?> (<?= $book['Publication_date']?>)
                </a>
            </li>
        <?php endforeach; ?>
    </ul>

</body> 
```
Listo!
