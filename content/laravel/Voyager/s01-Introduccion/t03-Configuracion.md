---
title: "Archivo de Configuración de Voyager"
menuTitle: "03. Archivo de Configuración de Voyager"
date: 2022-12-24T07:47:01-08:00
draft: false
weight: 15
---

### Donde esta el archivo de configuración
```php
config/voyager.php
```

### User config
```php
'user' => [
    'add_default_role_on_register' => true,
    'default_role'                 => 'user',
    'default_avatar'               => 'users/default.png',
    'redirect'                     => '/admin',
],
```

Por omisión Voyager ya asigna dos tipos de roles, uno de tipo usuario y otro de tipo admin, esto se ve en la tabla que genero llamada "roles".
La forma en que voyager pudo asignar rol de user es porque modifico el modelo app/Models/User.php para que ahora extienda de la clase \TCG\Voyager\Models\User.

Como podemos darnos cuenta en la tabla "users" Voyager a agregado un campo llamado "avatar" para almacenar una foto de perfil, pero por otro lado como estamos usando Jetstream también tenemos el campo de "profile_photo_path" para asignar foto a nuestros usuarios.

Vemos que la foto que se asigna en el campo de "avatar" es "users/default.png" que se encuentra en storage/app/public/users/default.png.

![Avatar](/Voyager/default.png)

Esta es la imagen que van a tener todos los usuarios una vez que se registren.

### Controllers config
Voyager nos proporciona de un Controlador el cual nos va a permitir realizar todas las operaciones de un CRUD.
Aquí podemos especificar la configuración del controlador.
```php
'controllers' => [
    'namespace' => 'TCG\\Voyager\\Http\\Controllers',
],
```

### Models config
Aquí podemos definir el namespace de nuestros modelos.
```php
'models' => [
    //'namespace' => 'App\\Models\\',
],
```
Vamos a des-comentar para que todos los modelos que vayamos a crear queden en App\Models\
```php
'models' => [
    'namespace' => 'App\\Models\\',
],
```

### Storage Config
Aquí tenemos que escoger un disco para almacenar todas las imagenes que subamos utilizando Voyager
```php
'storage' => [
    'disk' => 'public',
],
```
Porque Voyager no toma simplemente la variable de entorno que especificamos en .env (FILESYSTEM_DISK=public), porque es muy común que las imágenes que se suban con Voyager queden en un espacio distinto (Amazon S3, por Ejemplo), pero en este caso coincide que es el mismo destino "public".

### Database Config
No olvidar correr antes:
```php
lando npm run dev
```

Si nos dirigimos a:
```php
http://voyager.lndo.site/admin
```
Vemos el formulario de iniciar sesión.

![Formulario](/Voyager/iniciar-sesion.png)

Si vemos la tabla "users" vemos que Voyager a creado ya un usuario:
```php
name: Admin
email: admin@admin.com
password: password
```
Vamos a ingresar con estas credenciales.
Al entrar modificar los datos de este usuario, bajo el menu del usuario est ala opción profile, también especificar la opcion de locale a es para indicar que el ideoma ste en español.
```php
name: Enrique Sousa
email: enrique.sousa@gmail.com
password: Voyager1234
```

Podemos ver las tablas de la Base de Datos en:
http://voyager.lndo.site/admin/database

Como podemos ver algunas tablas NO se muestran, ya que no es necesario y son esenciales para el funcionamiento del paquete. En esta configuración podemos decidir que tablas van a estar escondidas:

```php
'database' => [
    'tables' => [
        'hidden' => ['migrations', 'data_rows', 'data_types', 'menu_items', 'password_resets', 'permission_role', 'personal_access_tokens', 'settings'],
    ],
    'autoload_migrations' => true,
],
```

### Dashboard config
Para agregar nuevos items a la lista dropdown "nav_items"
```php
'dashboard' => [
    // Add custom list items to navbar's dropdown
    'navbar_items' => [
        'voyager::generic.profile' => [
            'route'      => 'voyager.profile',
            'classes'    => 'class-full-of-rum',
            'icon_class' => 'voyager-person',
        ],
        'voyager::generic.home' => [
            'route'        => '/',
            'icon_class'   => 'voyager-home',
            'target_blank' => true,
        ],
        'voyager::generic.logout' => [
            'route'      => 'voyager.logout',
            'icon_class' => 'voyager-power',
        ],
    ],

    'widgets' => [
        'TCG\\Voyager\\Widgets\\UserDimmer',
        'TCG\\Voyager\\Widgets\\PostDimmer',
        'TCG\\Voyager\\Widgets\\PageDimmer',
    ],

],
```

**Que son los widgets**

![Widgets](/Voyager/voyager-widgets.png)

Son los recuadros informativos que Voyager nos muestra en la pagina de inicio, es esta sección es donde podemos controlar el desplegado de nuestros widgets.

### UI Generic Config
Para cambiar la tonalidad del color de la plantilla de Voyager
```php
'primary_color' => '#22A7F0',
```

También podemos especificar código custom css y js que afecte a toda nuestra plantilla de Voyager, aquí:
```php
// Here you can specify additional assets you would like to be included in the master.blade
'additional_css' => [
    //'css/custom.css',
],

'additional_js' => [
    //'js/custom.js',
],
```










