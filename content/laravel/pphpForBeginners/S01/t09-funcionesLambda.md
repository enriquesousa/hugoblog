---
date: 2023-03-01T09:17:14-08:00
title: "Funciones Lambda"
menuTitle: "09. Funciones Lambda"
draft: false
weight: 45
---

¡Abróchate el cinturón, porque tenemos mucho que cubrir en este episodio! Como parte de nuestra primera refactorización de código, analizaremos qué son las funciones lambda, así como cuándo y por qué podría utilizarlas.

Para poder filtrar por diferentes parámetros, no solo por autor si no por cualquier otro parámetro que tengamos en nuestro arreglo de $books, inicialmente nos veríamos tentados a empezar a duplicar código para filtrar por otro parámetro, pero para evitar esta duplicación de código, podemos aplicar las técnicas de las funciones lambda de php.  

## Que aprenderemos
- Lambdas
- Extract Variable
- array_filter


Inicialmente podríamos **extraer la variable** $filterBooks asi:
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
    $filteredBooks = filterByAuthor($books, 'Stephen King');
?>
    
<ul>
    <?php foreach ($filteredBooks as $book) : ?>
        <li>
            <a href="<?= $book['purchaseUrl'] ?>">
                <?= $book['name'] ?> (<?= $book['Publication_date']?>) - By <?= $book['author'] ?>
            </a>
        </li>
    <?php endforeach; ?>
</ul> 
```
A esto se le conoce como una **name function**
```php
function filterByAuthor($books, $author) 
```
Pero NO es obligatorio darle un nombre a la función!
Esto es muy interesante porque ahora sabemos que podemos crear tanto funcione con nombre (name functions) o funciones anónimas (Anonymous functions).

Las funciones anónimas, también conocidas como cierres (Closure), permiten la creación de funciones que no tienen un nombre específico. Son más útiles como el valor de los parámetros invocables, pero tienen muchos otros usos.

Las funciones anónimas se implementan utilizando la clase **Closure**.

Pero que podemos hacer con una función anónima?, pues la podemos asignar a variables o la podemos **pasar** a otras **funciones**. 
Por ejemplo para asignarle a una variable que sea igual a una función:
```php
$filterByAuthor = function ($books, $author) {
                      
}; 
```
Nota: como tenemos que terminar con un **';'** ahora!
Ahora tenemos una variable que apunta a una función.
Por lo tanto ya no tenemos la función con el nombre filterByAuthor() ahora para poder llamar la función anónima la llamamos con el nombre de la variable:
```php
$filteredBooks = $filterByAuthor($books, 'Andy Weir'); 
```
Nota: como se antepuso el carácter **'$'** en $filterByAuthor.
La función completa queda asi:
```php
<?php 
    // Función lambda para filtrar
    $filterByAuthor = function ($books, $author) {
        $filteredBooks = [];
        foreach ($books as $book) {
            if ($book['author'] === $author) {
                // append to the array
                $filteredBooks[] = $book; 
            }
        }
        return $filteredBooks;            
    };
    $filteredBooks = $filterByAuthor($books, 'Andy Weir');
?> 
```
Nota: A esto es lo que se le conoce con el nombre de **Lambda** function.

Ahora vamos a formar la función para que sea genérica.
Lo que queremos es tener una función donde le podamos pasar:
```php
filterByAuthor($books, key, value);
// por ejemplo
filterByAuthor($books, 'author', 'Andy Weir');
```
Ahora cambiarlos algunos nombres y parámetros de la funciones podemos decir:
```php
<?php 
    // Función lambda para filtrar
    function filter($items, $key, $value) {
        $filteredItems = [];

        foreach ($items as $item) {
            if ($item[$key] === $value) {
                // append to the array
                $filteredItems[] = $item; 
            }
        }

        return $filteredItems;            
    };
    
    $filteredBooks = filter($books, 'author','Andy Weir');
?> 
```
Listo!
Funciona!

Refactoring 1
Darle mas flexibilidad para poder filtrar rangos de publicationDate
Empezamos escribiendo el código que nos gustaría que funcionara:
```php
filter($books, function ($book){
    return $book[publicationDate] === 1968;
}); 
```  
Pasamos una función anónima! (Lambda), la función necesita aceptar el libro actual.
```php
<?php 
    // Función lambda para filtrar
    function filter($items, $fn) {
        $filteredItems = [];

        foreach ($items as $item) {
            if ($fn($item)) {
                $filteredItems[] = $item; 
            }
        }

        return $filteredItems;            
    };

    $filteredBooks = filter($books, function ($book){
        return $book['publicationDate'] === 1968;
    });
?> 
```
Listo!
Nota: Ya tenemos mas control sobre la función que podemos controlar mas los datos y fechas que deseamos filtrar, modificando simplemente los parámetros de la función lambda!, por ejemplo:
```php
$filteredBooks = filter($books, function ($book){
        return $book['publicationDate'] >= 2000;
    }); 

$filteredBooks = filter($books, function ($book){
        return $book['author'] === 'Andy Weir';
    });

$filteredBooks = filter($books, function ($book){
            return $book['purchaseUrl'] === 'http://example2.com';
        });
```
Listo!

Por ultimo, PHP nos proporciona **built-in** functions!

PHP tiene más de 1000 funciones integradas a las que se puede llamar directamente, desde un script, para realizar una tarea específica.

Consulte la referencia built-in functions de PHP para obtener una descripción completa de las funciones integradas visita este [link](https://www.w3schools.com/php/php_ref_overview.asp).

Para nuestro ejemplo anterior, podemos sustituir nuestra función:
```php
$filteredBooks = filter($books, function ($book){
        return $book['author'] === 'Andy Weir';
    });
```
Y la sustituimos por la función que ya viene con PHP asi:
```php
$filteredBooks = array_filter($books, function ($book){
        return $book['author'] === 'Andy Weir';
    }); 
```

## El código completo quedaría así:
{{< details "Código PHP" >}}

```php
<?php 
    // Este código ya no lo necesitamos
        // Función lambda para filtrar
        // function filter($items, $fn) {
        //     $filteredItems = [];

        //     foreach ($items as $item) {
        //         if ($fn($item)) {
        //             $filteredItems[] = $item; 
        //         }
        //     }

        //     return $filteredItems;            
        // };

        // $filteredBooks = filter($books, function ($book){
        //     return $book['purchaseUrl'] === 'http://example2.com';
        // });


    // Mandar llamar la **built-in** function de PHP mejor! 
    $filteredBooks = array_filter($books, function ($book){
        return $book['author'] === 'Andy Weir';
    });    
?>
    
<ul>
    <?php foreach ($filteredBooks as $book) : ?>
        <li>
            <a href="<?= $book['purchaseUrl'] ?>">
                <?= $book['name'] ?> (<?= $book['publicationDate']?>) - By <?= $book['author'] ?>
            </a>
        </li>
    <?php endforeach; ?>
</ul> 
```

{{< /details >}}


**Listo!**  {{< line_break >}}
Vemos como nos ahorramos todo el código que hicimos!
