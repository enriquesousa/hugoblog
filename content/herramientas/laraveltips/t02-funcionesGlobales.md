---
title: "Funciones Globales"
date: 2024-05-17T07:11:38-07:00
draft: false
---

## Objetivo
Crear funciones globales que podamos llamar de cualquier parte de nuestro código.
<br>

## Crear Archivo
Crear archivo donde vamos a almacenar nuestras funciones.
`En app/Funciones/globales.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
<?php


// Convertir Currency a Numero Float
function getAmount($money)
{
    $cleanString = preg_replace('/([^0-9\.,])/i', '', $money);
    $onlyNumbersString = preg_replace('/([^0-9])/i', '', $money);

    $separatorsCountToBeErased = strlen($cleanString) - strlen($onlyNumbersString) - 1;

    $stringWithCommaOrDot = preg_replace('/([,\.])/', '', $cleanString, $separatorsCountToBeErased);
    $removedThousandSeparator = preg_replace('/(\.|,)(?=[0-9]{3,}$)/', '',  $stringWithCommaOrDot);

    return (float) str_replace(',', '.', $removedThousandSeparator);
}

?>
```
</details>
<br>

## Registrar
Ahora registrar el archivo de funciones globales bajo la sección de auto load.
`En composer.json`
<details>
  <summary>Click para ver Código. </summary>
  
```
...
"autoload": {
	"psr-4": {
		"App\\": "app/",
		"Database\\Factories\\": "database/factories/",
		"Database\\Seeders\\": "database/seeders/"
	},
	"files": [
		"app/Funciones/globales.php"
	]
},
...
```
</details>
<br>

Como le hicimos cambios a composer, tenemos que correr:
`composer dump-autoload`

## Colisiones
Para asegurarnos que no exista otra funcion con el mismo nombre y pueda existir alguna colision, protegemos nuestra función con:
`En app/Funciones/globales.php`
{{< details "Texto" >}}
    if(!function_exists('getAmount')){

        // Convertir Currency a Numero Float
        function getAmount($money)
        {
            $cleanString = preg_replace('/([^0-9\.,])/i', '', $money);
            $onlyNumbersString = preg_replace('/([^0-9])/i', '', $money);

            $separatorsCountToBeErased = strlen($cleanString) - strlen($onlyNumbersString) - 1;

            $stringWithCommaOrDot = preg_replace('/([,\.])/', '', $cleanString, $separatorsCountToBeErased);
            $removedThousandSeparator = preg_replace('/(\.|,)(?=[0-9]{3,}$)/', '',  $stringWithCommaOrDot);

            return (float) str_replace(',', '.', $removedThousandSeparator);
        }
        
    }
{{< /details >}}
<br>



* * *
Listo!

