---
title: "Relation One To Many"
date: 2024-05-20T07:15:16-07:00
draft: false
---

## Relación uno a muchos
*Ahora desde el punto de vista de la tabla de las categorías, podemos crear una relación de una categoría con muchos post de la tabla de blogs.*
<br>

## Modelo Category
En el modelo creamos la relacion.
`En app/Models/Category.php`
Ahora de los dos tipos de relacion que nos ofrece laravel `hasMany` o `belogsToMany`, el que necesitamos usar es *hasMany*:
<details>
  <summary>Click para ver Código. </summary>
  
```
class Category extends Model
{
    use HasFactory;
    
    public function blogs(){
        return $this->hasMany(Blog::class);
    }
}
```
</details>
<br>

## Para desplegar los posts
Si buscamos uno de los posts y ese post tiene una categoría que existe en otros posts entonces veremos todos los posts donde este esa misma categoría.
<details>
  <summary>Click para ver Código. </summary>
  
```
$category = Category::find(1);
return $category->blogs;
```
</details>
<br>

Lo podemos hacer con `eager` loading.
<details>
  <summary>Click para ver Código. </summary>
  
```
$category = Category::with('blogs')->find(1);
return $category;
```
</details>
<br>


* * *
Listo.
