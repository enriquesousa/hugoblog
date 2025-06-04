---
title: "Mostrar y Agregar Atributos"
date: 2025-06-04T11:10:23-07:00
draft: false

weight: 15
---

## Mostrar atributos usando val()

`En index.html`
*Código:*
```php
...
<input type="text" placeholder="Usuario">
<input type="password" placeholder="Contraseña">
<button id="boton">Enviar</button>

<script src="js/mostrarAtributos.js"></script>
...
```

`En js/mostrarAtributos.js`
*Código:*
```php
var usuario = "";
var password = "";

$("#boton").click(function() {

    usuario = $("[type='text']").val();
    password = $("[type='password']").val();

    console.log("Usuario: " + usuario);
    console.log("Password: " + password);
});
```

Al dar click al botón Enviar vemos en la consola los atributos.
![imagen](/herramientas/jquery/image_20250604074018.png)

## Agregar un atributo usando attr()

`En index.html`
*Código:*
```php
<div id="caja2"></div>

<script src="js/agregarAtributo.js"></script>
```
vamos agregar un atributo a caja2.

Crear una caja2 amarilla donde podamos hacer click.
`En css/estilo.css`
*Código:*
```php
#caja2 {
  position: relative;
  width: 50px;
  height: 50px;
  background-color:rgb(233, 223, 46);
  margin-top: 20px;
  cursor: pointer;  
}
```

Agregar el atributo personalizado con jquery.
`En js/agregarAtributo.js`
*Código:*
```php
$("#caja2").click(function() {
    $(this).attr("jose", "es un atributo personalizado");
});
```

Al dar click en la caja amarilla vemos.
![imagen](/herramientas/jquery/image_20250604110420.png)

vemos el cambio de atributo.
![imagen](/herramientas/jquery/image_20250604110708.png)


***
Documentación | TJWeb | JQuery | Mostrar y Agregar Atributos

