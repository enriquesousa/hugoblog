---
title: "Eloquent Relation One To One"
date: 2024-05-18T17:04:40-07:00
draft: false
---


## Relación uno a uno
*Para la relación uno a uno*
<br>


## Crear Nuevo modelo 
Creamos un nuevo modelo con su tabla de migración.
`php artisan make:model Category -m`
<br>

## Tabla Campos
Crear los campos.
<details>
  <summary>Click para ver Código. </summary>
  
```
Schema::create('categories', function (Blueprint $table) {
	$table->id();

	$table->string('name');

	$table->timestamps();
});
```
</details>
<br>

## Migrar
`php artisan migrate`
<br>

## Agregar la Llave Foránea
Agregar la llave foránea. Usamos convención de laravel. `$table->foreignId('category_id')->constrained('categories');` como podemos ver tiene que ser el nombre del modelo con minúsculas un guion bajo y id.
`En database/migrations/2024_05_18_163224_create_blogs_table.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
Schema::create('blogs', function (Blueprint $table) {
	$table->id();
	$table->foreignId('category_id')->constrained('categories');

	$table->string('title');
	$table->text('body');
	$table->boolean('status');

	$table->timestamps();
});
```
</details>
<br>
	
## Migrar con fresh
`php artisan migrate:fresh`
<br>

## Orden de las migraciones
Para poder ejecutar la orden `$table->foreignId('category_id')->constrained('categories');` tenemos que:
Asegurarnos que la migración de la tabla de categories este antes de la de blogs.
![img](/laraveltips/relationOneToOne/relationOneToOne-1.png)
Si ya creamos la tabla blogs antes de la tabla de categories, lo que ppodemos hacer es cambiar el tiempo en el nombre de las migraciones para que quede primero la tabla de categories antes de la tabla de blogs.
<br>

## Ventajas de Usar foreignId()
Una de las ventajas de usar foreignId() es que ya queda registrado en la base de datos la relación.
![img](/laraveltips/relationOneToOne/relationOneToOne-2.png)
<br>

## Crear algunas Categorías
Crear algunas categorías manualmente.
1. Tech
2. Sports
3. Education 

![img](/laraveltips/relationOneToOne/relationOneToOne-3.png)
<br>

## Crear algunos blogs
Manualmente.
![img](/laraveltips/relationOneToOne/relationOneToOne-4.png)
<br>

## Crear la relación en Modelo
Un post solo puede tener una categoría por eso podemos usar una relacion uno a uno.
Hay dos tipos de relacioni uno a uno que podemos usar en laravel, `hasone` y `belogsto` , en nuestro caso sabemos que `category_id` *belogs to* an *id* en la tabla de `categories`.
`En app/Models/Blog.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
class Blog extends Model
{
    use HasFactory;

    protected $table = 'blogs';

    
    public function category(){
        return $this->belongsTo(Category::class);
    }
    
}

```
</details>
<br>

## Para Desplegar el Nombre de la Categoría
En el controlador podemos colocar código para que nos regresa el objeto.
`En app/Http/Controllers/BlogController.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
public function index()
{
	$blog = Blog::find(1);
	$nombreCategoria = $blog->category;

	// return $blog;
	return $nombreCategoria;
	// return $nombreCategoria->name;
}
```
</details>

**Objeto $blog Regresa:**
<details>
  <summary>Click para ver Código. </summary>
  
```
id: 1,
category_id: 1,
title: "Este es un titulo Tech",
body: "Este es un body Tech",
status: 1,
created_at: null,
updated_at: null,
category: {
id: 1,
name: "Tech",
created_at: null,
updated_at: null
}
```
</details>
<br>

**Objeto $nombreCategoria Regresa:**
<details>
  <summary>Click para ver Código. </summary>
  
```
id: 1,
name: "Tech",
created_at: null,
updated_at: null
```
</details>
<br>


**Objeto $nombreCategoria->name Regresa:**
<details>
  <summary>Click para ver Código. </summary>
  
```
Tech
```
</details>
<br>
<br>

## Para Get todos los blog incluyendo todas sus categorías.
Para Get todos los blog incluyendo todas sus categorías.
Lo que podemos usar el eger loading de laravel donde también hacemos fetch de los objetos relacionados. Usamos el método with().
<details>
  <summary>Click para ver Código. </summary>
  
```
$blog = Blog::with('category')->get();
return $blog;
```
</details>
<br>

**Obtenemos:**
<details>
  <summary>Click para ver Código. </summary>
  
```
{
id: 1,
category_id: 1,
title: "Este es un titulo Tech",
body: "Este es un body Tech",
status: 1,
created_at: null,
updated_at: null,
category: {
id: 1,
name: "Tech",
created_at: null,
updated_at: null
}
},
{
id: 2,
category_id: 2,
title: "Este es Titulo Sports",
body: "Este es Body Sports",
status: 1,
created_at: null,
updated_at: null,
category: {
id: 2,
name: "Sports",
created_at: null,
updated_at: null
}
},
{
id: 3,
category_id: 3,
title: "Este es titulo  Education",
body: "Este es body  Education",
status: 1,
created_at: null,
updated_at: null,
category: {
id: 3,
name: "Education",
created_at: null,
updated_at: null
}
}
```
</details>
<br>

## Desplegar solo titulos
Ya como tenemos los objetos que necesitamos podemos.
`En app/Http/Controllers/BlogController.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
$blog = Blog::with('category')->get();

foreach ($blog as $key => $value) {
	echo $value->title;
	echo ' - ';
	echo $value->category->name;
	echo '<br>';
}
```
</details>
<br>


![img](/laraveltips/relationOneToOne/relationOneToOne-5.png)

* * *
Listo.
