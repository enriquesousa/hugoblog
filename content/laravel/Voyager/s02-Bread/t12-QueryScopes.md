---
title: "Query Scopes"
menuTitle: "12. Query Scopes"
date: 2022-12-24T07:47:01-08:00
draft: false
weight: 45
---

### Query Scopes
Nos ayudan para filtrar que solo podamos ver y editar los productos que están a nombre de la persona que esta logiada.
Agregar código en app/Models/Product.php:
```php
 <?php
namespace App\Models;
use Illuminate\Database\Eloquent\Model;
class Product extends Model
{
    // Con esto filtramos que solo el user que esta log in pueda ver solo productos ue están a su nombre
    public function scopeCurrentUser($query)
    {
        $query->where('user_id', auth()->user()->id);
    }
}
```
El siguiente paso es modificar el parámetro de "Alcance" en BREAD/products a quedar en "currentUser", nota como ya en forma automática Voyager ya nos detecta la función de scope.

Esta es la forma que tenemos para restringir la información que podemos ver.
