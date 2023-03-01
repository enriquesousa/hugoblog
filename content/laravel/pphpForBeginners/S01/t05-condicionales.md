---
date: 2023-03-01T09:15:33-08:00
title: "Condicionales y Booleanos"
menuTitle: "05. Condicionales"
draft: false
weight: 25
---

## Desplegar un titulo de un libro
Cambiamos un poco el estilo css del body tag para que se vea un poco mas estético. 
{{< details "Código PHP" >}}
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Learn PHP</title>
    <style>
        body {
            display: grid;
            place-items: center;
            height: 100vh;
            margin: 0;
            font-family: sans-serif;
        }
    </style>
</head>

<body>
    
    <?php
        $name = "Dark Matter";
    ?>

    <h1>
        Tu haz leído "<?php echo $name; ?>."
    </h1>    
</body>

</html> 
```
{{< /details >}}


## Primer condicional
Ahora vamos a aprender nuestro primer condicional.
```php
<?php
    $name = "Dark Matter";
    $read = true;
    if ($read) {
        $message = "Tu haz leído $name";
    }else {
        $message = "Tu NO haz leído $name";
    }
?>

<h1>
    <?php echo $message; ?>
</h1>
```
Refactoring 1
```php
<h1>
    <?= $message ?>
</h1> 
```

