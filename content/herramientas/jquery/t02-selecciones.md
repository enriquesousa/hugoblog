---
title: "Selecciones"
date: 2025-06-03T10:26:06-07:00
draft: false

weight: 10
---

## Selección This

`En index.html`
*Código:*
```php
<div class="cajas"></div>
<div class="cajas"></div>

<script src="js/seleccionThis.js"></script>
```

`En css/estilo.css`
*Código:*
```php
.cajas {
  position: relative;
  width: 100px;
  height: 100px;
  background-color: rgb(109, 221, 57);
  cursor: pointer;
  margin: 20px;
}
```

`En js/seleccionThis.js`
*Código:*
```php
// Si damos click en un elemento con la clase "cajas", cambia su color de fondo a amarillo
$(".cajas").click(function() {
    $(this).css("background-color", "yellow");
});
```

![imagen](/herramientas/jquery/Pasted_image_20250603095321.png)

Si le damos click al segundo cuadro:
![imagen](/herramientas/jquery/Pasted_image_20250603095404.png)
De esta manera optimizamos código para dar click en una caja en especifico.

## Selección Tipo Atributo

`En index.html`
*Código:*
```php
<!-- Selección Atributos -->
<input type="text" placeholder="Usuario">
<input type="password" placeholder="Contraseña">
<button id="boton">Enviar</button>
```

`En js/seleccionAtributos.js`
*Código:*
```php
$("#boton").click(function() {
    $("[type='text']").css("background-color", "yellow");
});
```

Si le damos click a enviar la caja de Usuario se tiene que pintar.
![imagen](/herramientas/jquery/Pasted_image_20250603102311.png)

![imagen](/herramientas/jquery/Pasted_image_20250603102332.png)

***
Documentación | TJWeb | JQuery | Selecciones
