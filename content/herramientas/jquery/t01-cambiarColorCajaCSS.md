---
title: "Cambiar Color en Caja CSS"
date: 2025-06-02T11:24:26-07:00
draft: false

weight: 5
---

## Cambiar de Color una Caja CSS
Estructura básica de Jquery para cambiar de color a una caja css en index.html.

Librería de JQuery [link](https://jquery.com/download/)
Bajar la version comprimida. Download the compressed, production version. [link](https://code.jquery.com/jquery-3.7.1.min.js)

JQuery UI [link](https://jqueryui.com)

JQuery Mobile [link](https://jquerymobile.com)

JQuery Plugins [link](https://jquery-plugins.net)

## Bajar el archivo
Bajar el archivo `jquery-3.7.1.min.js` y guardarlo en:
`/home/enrique/Sites/Tests/JQUERY/jquery/js/jquery-3.7.1.min.js`

`En index.html`
*Código:*
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Introducción a JQuery</title>

    <link rel="stylesheet" href="css/estilo.css">

    <script src="js/jquery-3.7.1.min.js"></script>

</head>
<body>
    
    <div id="caja"></div>

    <script src="js/codigo.js"></script>
</body>
</html>
```

`En css/estilo.css`
*Código:*
```php
.body {
  margin: 0;
    padding: 0;
    list-style: none;
    text-decoration: none;
    font-family: Arial, sans-serif;
}

#caja {
  width: 200px;
  height: 100px;
  background-color: blue;
  cursor: pointer;
}
```

`En js/codigo.js`
*Código:*
```php
$(document).ready(function() {
    $("#caja").click(function() {
        $(this).css("background-color", "red");
    });
});
```

![imagen](/herramientas/jquery/Pasted_image_20250602101915.png)

![imagen](/herramientas/jquery/Pasted_image_20250602101928.png)

## Podemos agregar mas parámetros

`En js/codigo.js`
*Código:*
```php
$(document).ready(function() {
    $("#caja").click(function() {
        $(this).css(
            {
                "background-color": "yellow",
                "width": "600px",
                "height": "100px"
            }
        );
    });
});
```

![imagen](/herramientas/jquery/Pasted_image_20250602102913.png)



***
Documentación | TJWeb | JQuery | Cambiar de Color una Caja CSS
