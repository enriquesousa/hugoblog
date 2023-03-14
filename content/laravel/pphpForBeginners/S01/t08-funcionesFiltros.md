---
date: 2023-03-01T09:16:43-08:00
title: "Funciones y Filtros"
menuTitle: "08. Funciones y Filtros"
draft: false
weight: 40
---

¡Felicitaciones por llegar tan lejos! Vayamos un paso más allá ahora y revisemos las funciones. Puedes pensar en las funciones como los verbos del mundo de la programación.

## Que aprenderemos
- Functions
- Array Filtering
- Checking for Equality


Filtrar por autor
```php
<?php foreach ($books as $book) : ?>
    <?php if($book['author'] == 'Andy Weir') : ?>
        <li>
            <a href="<?= $book['purchaseUrl'] ?>">
                <?= $book['name'] ?> (<?= $book['Publication_date']?>)
            </a>
        </li>
    <?php endif; ?>
<?php endforeach; ?> 
```

Operadores comparativos php
```php
$a == $b 	Equal 	                true if $a is equal to $b after type juggling.
$a === $b 	Identical 	            true if $a is equal to $b, and they are of the same type.
$a != $b 	Not equal 	            true if $a is not equal to $b after type juggling.
$a <> $b 	Not equal 	            true if $a is not equal to $b after type juggling.
$a !== $b 	Not identical 	        true if $a is not equal to $b, or they are not of the same type.
$a < $b 	Less than 	            true if $a is strictly less than $b.
$a > $b 	Greater than 	        true if $a is strictly greater than $b.
$a <= $b 	Less than or equal to 	true if $a is less than or equal to $b.
$a >= $b 	Greater than or equal   true if $a is greater than or equal to $b.
$a <=> $b 	Spaceship 	An int less than, equal to, or greater than zero when $a is less than, equal to, or greater than $b, respectively.  
```

Crear una función para filtrar, en su forma mas sencilla
```php
<body>
    <h1>Libros Recomendados</h1>
    <?php
        
        $books = [];

        // Función para filtrar
        function filterByAuthor() {
            return 'Hola'; 
        }

    ?>

    <p>
        <?= filterByAuthor(); ?>
    </p>
</body> 
```
Ahora vamos a pasarle parámetros.
La forma larga
```php
<?php 
    // Función para filtrar
    function filterByAuthor($books) {
        $filteredBooks = [];
        foreach ($books as $book) {
            if ($book['author'] === 'Andy Weir') {
                // append to the array
                $filteredBooks[] = $book; 
            }
        }
        return $filteredBooks;            
    }
?>
    
<ul>
    <?php foreach (filterByAuthor($books) as $book) : ?>
        <li>
            <a href="<?= $book['purchaseUrl'] ?>">
                <?= $book['name'] ?> (<?= $book['Publication_date']?>) - By <?= $book['author'] ?>
            </a>
        </li>
    <?php endforeach; ?>
</ul> 
```
Refactor 1
Ahora pasar el valor del autor como parámetro
```php
<?php 
    // Función para filtrar
    function filterByAuthor($books, $author) {
        $filteredBooks = [];
        foreach ($books as $book) {
            if ($book['author'] === $author) {
                // append to the array
                $filteredBooks[] = $book; 
            }
        }
        return $filteredBooks;            
    }
?>
    
<ul>
    <?php foreach (filterByAuthor($books, 'Stephen King') as $book) : ?>
        <li>
            <a href="<?= $book['purchaseUrl'] ?>">
                <?= $book['name'] ?> (<?= $book['Publication_date']?>) - By <?= $book['author'] ?>
            </a>
        </li>
    <?php endforeach; ?>
</ul> 
```
Listo!

En la siguiente lección veremos una forma mas avanzada de mejorar este código!

Usando las funciones dedicadas de PHP llamadas `Lambda`.
