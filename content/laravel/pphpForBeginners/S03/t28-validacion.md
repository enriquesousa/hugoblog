---
date: 2023-03-13T17:42:19-07:00
title: "Extraer las clases de Validación"
menuTitle: "28. Validator Class"
draft: false
weight: 50
---

Para lograr una experiencia más flexible y legible, extraigamos las reglas de validación básicas que escribimos en el episodio anterior en una clase Validación dedicada.

## Que aprenderemos
- Validation
- Pure Functions
- Static Methods

## Crear la clase
Para requerir una clase puede se de dos maneras, cualquiera de las dos es valida.
```php
require 'Validator.php';
require('Validator.php');
```
Crear la clase `Validator.php` en la raíz del proyecto.
Recuerda que por convención capitalizamos la `V` en `Validator.php` para indicar que este archivo solo va a tener una sola clase.
```php
class Validator
{
    public function string($value, $min = 1, $max = INF)
    {
        $value = trim($value);
        return strlen($value) >= $min && strlen($value) <= $max;
    }
}
```

y en `controllers/note-create.php`
```php
$errors = [];
$validator = new Validator();
if (! $validator->string($_POST['body'], 1, 1000) ) {
	$errors['body'] = 'Se requiere contenido entre 1 y 1000 caracteres.';
}
```

## Análisis
INF = Infinite

## Funciones puras y Static Functions
Una función pura es cuando no tenemos dentro de ella referencias a propiedades internas de la clase o a otras clases, simplemente recibe unos parámetros y regresa una respuesta, cuando tenemos funciones puras podemos declararlas `static`. Al definir una función static nos permite mandarla llamar sin necesidad de declarar una instancia de la clase.

Entonces nos quedan así los archivos:
En `controllers/note-create.php`:
```php
$errors = [];
if (! Validator::string($_POST['body'], 1, 1000) ) {
	$errors['body'] = 'Se requiere contenido entre 1 y 1000 caracteres.';
}
```
y static en `Validator.php`:
```php
public static function string($value, $min = 1, $max = INF){
	$value = trim($value);
	return strlen($value) >= $min && strlen($value) <= $max;
}
```

Como un extra vamos a ver como validar un email.
Vamos a usar un [función de php](https://www.php.net/manual/en/function.filter-var.php).
(PHP 5 >= 5.2.0, PHP 7, PHP 8)
`filter_var` — Filters a variable with a specified filter
En `Validator.php`:
```php
public static function email($value){
	return filter_var($value, FILTER_VALIDATE_EMAIL);
}
```

En cuestión de funcionalidad quedamos como al final de la lección pasada, pero ahora, estamos en una mucho mejor posición, en donde podemos mandar llamar a nuestro `Validator` de cualquier parte para que haga las funciones correspondientes de validar nuestros campos. 

Listo!



