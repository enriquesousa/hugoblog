---
date: 2023-03-01T09:15:08-08:00
title: "Tu primera etiqueta PHP"
menuTitle: "03. Etiqueta PHP"
draft: false
weight: 15
---

**Acerca de esta lección**: Nuestra primera orden del día es preparar algo de HTML básico, iniciar un servidor PHP y verlo en el navegador.

## Que aprenderemos
- PHP Tags
- Strings
- echo

## Tarea
* * *
Cree un párrafo que use PHP para hacer eco de cualquier oración básica de su elección. Practique escribir las etiquetas <?php de apertura y cierre.

## Crear primer archivo
En `index.php` en alguna carpeta por ejemplo learn-php/
Empezamos con el clásico "Hola, Mundo!"

<details>
  <summary>Click to see code. </summary>
  
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>
        <?php  
            echo "Hola, Mundo!";
        ?>
    </h1>    
</body>
</html> 
```
</details>

## PHP Help
Si corremos en la terminal.
```php
php --help 
```
php nos dará varias opciones de operación, de ahi la que nos interesa en este momento es la bandera -S.

## PHP Server
Para servirlo podemos usar directamente PHP con:
```php
php -S localhost:8888 
```

