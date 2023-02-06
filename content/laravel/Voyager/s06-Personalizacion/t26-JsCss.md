---
title: "JS y CSS Adicional"
menuTitle: "26. JS y CSS Adicional"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 10
---

### Estilos CSS y JS para todas las plantillas de Voyager
Crear dos carpetas:
```php
public/css/custom.css
public/js/custom.js 
```
Para incluir estos dos archivos dentro de la plantilla de Voyager, en config/voyager.php
```php
'additional_css' => [
    'css/custom.css',
],

'additional_js' => [
    'js/custom.js',
], 
```
Desde este momento hemos agregado archivos adicionales a la plantilla Voyager para poder agregar nuestros propios código de js y css, por ejemplo:
en public/js/custom.js:
```php
alert('Hola desde el archivo custom.js'); 
```
Podemos recargar la pagina y vemos la alerta!!

en public/css/custom.css:
```php
body            
{
    margin:auto;
    width:1024px;
    padding:10px;
    background-color:#ebebeb;
    font-size:14px;
    font-family:Verdana;
} 
```
Solo como ejemplo.
