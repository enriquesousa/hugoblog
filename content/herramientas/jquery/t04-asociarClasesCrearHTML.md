---
title: "Asociar Clases y Crear HTML"
date: 2025-06-06T15:43:43-07:00
draft: false

weight: 20
---


`En index.html`
*Código:*
```php
<button id="boton1">Asociar Clase</button>
<button id="boton2">Quitar Clase</button>

<p>Lorem ipsum dolor sit amet consectetur adipisicing elit. At ea nemo adipisci voluptates eveniet sequi quibusdam dolor rerum odit saepe non praesentium ab, fuga quidem laborum voluptatibus ad repellendus et!</p>

<!-- Scripts -->
<script src="js/asociarClases.js"></script>
```

`En css/estilo.css`
*Código:*
```php
.parrafo{
  color: white;
  font-size: 20px;
  background: green;
  padding: 20px;
  text-align: right;
}
```

`En js/asociarClases.js`
*Código:*
```php
$('#boton1').click(function() {
    $('p').addClass('parrafo');
});

$('#boton2').click(function() {
    $('p').removeClass('parrafo');
});
```

![imagen](/herramientas/jquery/image_20250606152036.png)

## Crear HTML

`En index.html`
*Código:*
```php
<button id="boton3">Agregar Caja HTML</button>
<div id="contenedor">
	<div id="c1">Hola</div> 
</div>

<!-- Scripts -->
<script src="js/crearHTML.js"></script>
```

`En js/crearHTML.js`
*Código:*
```php
$("#boton3").click(function() {
    // Reemplazar contenido html
    $('#contenedor').html('<div id="c1">Adios</div> ');
});
```

![imagen](/herramientas/jquery/image_20250606153500.png)

Al dar click al botón.
![imagen](/herramientas/jquery/image_20250606153533.png)
Nos reemplaza el html dentro del div contenedor.

***
Documentación | TJWeb | JQuery | Asociar Clases y Crear HTML