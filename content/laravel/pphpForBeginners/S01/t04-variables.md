---
date: 2023-03-01T09:15:19-08:00
title: "Variables"
menuTitle: "04. Variables"
draft: false
weight: 20
---

Bien, avancemos y revisemos la concatenación básica y las variables. La primera vez que aprendí sobre las variables, mi primer pensamiento fue: "¿Pero por qué?". ¡Hablemos de eso!

## Que aprenderemos
- Concatenation
- Variables

## Concatenar strings.
## Modo 1
```php
<h1>
    <?php  
        echo "Hola, "."Mundo!";
    ?>
</h1>  
```
## Modo 2
```php
<?php  
    $saludo = "Hola";
    echo $saludo." "."Mundo!";
?> 
```
## Refactoring 1
```php
<?php  
    $saludo = "Hola";
    echo "$saludo Mundo!";
?> 
```

# Definición de Refactoring
Cuando modificamos el código sin necesariamente modificar el resultado para el usuario final!

Ahora si cambiamos los " por '
```php
<?php  
    $saludo = "Hola";
    echo '$saludo Mundo!';
?> 
```
Literalmente tenemos a la salida el string "$saludo Mundo!"
y esto no es lo que queremos!
los single '' tienen su uso que los veremos mas adelante.