---
title: "Controladores"
menuTitle: "04. Controladores"
date: 2022-10-22T18:24:43-07:00
draft: false
weight: 20
---

### Porque tenemos la necesidad de crear un Controlador?
Laravel se basa en un modelo

### Modelo, Vista, Controlador.

**MVC** (modelo, vista, controlador) es un patrón arquitectónico de software que separa una aplicación en tres capas descritas como su acrónimo lo indica. Laravel, así como la mayoría de frameworks en PHP implementan este patrón de diseño en donde cada capa maneja un aspecto de la aplicación. Pero antes de ver cómo Laravel está diseñado para implementar este patrón de software, vamos a tratar de dejar este concepto un poco más claro definiendo cada una de sus partes.

**Modelo:** Hace referencia a la estructura de datos de la aplicación. Los datos pueden ser transferidos desde la base de datos, una clase, un servicio, u otros, directamente a la vista o ser transformados en el controlador para ser actualizados nuevamente al origen.

**Vista:** Es la representación de la información en una interfaz de usuario. Por lo general en interfaces no estáticas se representan los datos que vienen directamente del modelo o estos son transformados en un proceso intermedio en el controlador. En vistas estáticas por lo general no hace falta que las vistas sean renderizadas con datos enviados del controlador.

**Controlador:** Es el lugar en donde se implementa la lógica de la aplicación, los procedimientos, algoritmos y rutinas que hacen que funcione el software. Actúa como interfaz entre los componentes de modelo y vista aplicando las transformaciones y lógica necesarias.

![[Pasted image 20221112105554.png]]


*Ejemplo:* Crear un controlador "HomeController" para que nos despliegue el mensaje de bienvenida a la pagina!
- php artisan make:controller HomeController

En: app/Http/Controllers/Controller.php
```php
<?php
namespace App\Http\Controllers;
use Illuminate\Http\Request;

class HomeController extends Controller
{
	public function __invoke(){
	// return view('welcome');
	return "Bienvenido Pagina Principal! desde un Controlador";
	}
}
```

El método es cuando se va administrar una única ruta!
