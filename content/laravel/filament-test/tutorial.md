---
title: "Probando Filament PHP"
date: 2023-02-24T16:31:23-08:00
draft: false
weight: 5
menuTitle: "Test Run"
---


## Filament YouTube Tutorial
[YouTube](https://www.youtube.com/watch?v=In8SiXqMwh0)

**Construir Recursos**
https://filamentphp.com/docs/2.x/admin/resources/getting-started


### Crear nuevo proyecto de laravel
```php
laravel new filamentPrueba 
cd filamentPrueba
npm install
```

### Correr server y dev
```php
php artisan serve 
```
y en otra terminal
```php
npm run dev 
```

### Crear base de datos
Para simplificar las cosas usaremos SQLite
en **.env**
```php
DB_CONNECTION=sqlite
# DB_CONNECTION=mysql
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=filamentprueba
# DB_USERNAME=root
# DB_PASSWORD= 
```
Y crear el archivo **database.sqlite** en database/database.sqlite

### Hacer las Migraciones
```php
php artisan migrate 
```

### Instalar [filament](https://filamentphp.com/docs/2.x/admin/installation#installation)
Para comenzar con el panel de administración, puede instalarlo usando el comando:
```php
composer require filament/filament:"^2.0" 
```

### Crear usuario de filament
```php
php artisan make:filament-user 
```

**Credenciales**
```php
user: admin
admin@admin.com
admin1234 
```
**Success!**<br>
admin@admin.com may now log in at http://localhost:8000/admin/login

### Crear un Modelo Post
Crear primero modelo Post con su migración
```php
php artisan make:model Post -m 
```

Añadir dos campos a la tabla que se creo.
En database/migrations/2023_02_24_045358_create_posts_table.php
```php
Schema::create('posts', function (Blueprint $table) {
    $table->id();

    $table->string('title');
    $table->string('body');

    $table->timestamps();
}); 
``` 

Y proteger los campos en el Modelo **Post**
En app/Models/Post.php
```php
class Post extends Model
{
    use HasFactory;
    protected $guarded = [];
} 
```

Los recursos son clases estáticas que se utilizan para construir interfaces **CRUD** para sus modelos Eloquent. Describen cómo los administradores deberían poder interactuar con los datos de su aplicación, utilizando tablas y formularios.
```php
php artisan make:filament-resource Post 
```

Volver hacer las migraciones
```php
php artisan migrate 
```

Ya tenemos el post en la url ```http://127.0.0.1:8000/admin/posts```
Para poder empezar a crear unos post tenemos que llenar la forma en app/Filament/Resources/PostResource.php
```php
use Filament\Forms\Components\RichEditor;

public static function form(Form $form): Form
{
    return $form
        ->schema(
            [
            Forms\Components\TextInput::make('title')->label('Titulo')->required(),
            Forms\Components\RichEditor::make('body')->label('Contenido')->required(),
            ]
        );
} 
```

Ahora Crear algunos post con el botón "Crear" y al volver a los posts vemos que no vemos ninguno desplegado en la tabla!
Para poder ver los datos de los post, tenemos que agregar una tabla, la ver documentación de filament [tablas](https://filamentphp.com/docs/2.x/tables/columns/text#displaying-a-description)

Agregar código al método table() en app/Filament/Resources/PostResource.php
```php
public static function table(Table $table): Table
{
    return $table
        ->columns([
            Tables\Columns\TextColumn::make('title'),
            Tables\Columns\TextColumn::make('body')->html(),
        ])
        ->filters([
            //
        ])
        ->actions([
            Tables\Actions\EditAction::make(),
        ])
        ->bulkActions([
            Tables\Actions\DeleteBulkAction::make(),
        ]);
} 
```
Nota: usamos ```->html()``` para poder ver adecuadamente el texto rich text.

Por ultimo para poder ver los componentes:
```php
Forms\Components\TextInput::make('title')->label('Titulo')->required(),
Forms\Components\RichEditor::make('body')->label('Contenido')->required(), 
```
En forma mas estética, esto es que los dos componentes se encuentren en la misma columna, usamos el método:
```php
Card::make()->columns(1) 
``` 
Pasando el parámetro de 1 columna solamente!
El código del método form(Form $form) queda asi:
```php
use Filament\Forms\Components\RichEditor;
use Filament\Forms\Components\Card;

public static function form(Form $form): Form
{
    return $form
        ->schema(
            Card::make()->columns(1)->schema(
                [
                Forms\Components\TextInput::make('title')->label('Titulo')->required(),
                Forms\Components\RichEditor::make('body')->label('Contenido')->required(),
                ]
            )
    );
} 
``` 
No olvidar incluir:
```php
use Filament\Forms\Components\RichEditor;
use Filament\Forms\Components\Card; 
```




