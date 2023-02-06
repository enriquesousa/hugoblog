---
title: "Instalar Voyager"
menuTitle: "01. Instalar Voyager"
date: 2022-12-24T07:47:01-08:00
draft: false
weight: 5
---

### Paso 1
**Instalar Proyecto nuevo de Laravel**

Iniciar un proyecto nuevo de laravel con ambiente de desarrollo **lando**.

**Nota!**
```php
No hacer las migraciones todavía
```

Usando los 10 pasos explicados [Aquí]({{< ref "/laravel/Ecommerce/s01-introduccion/t02-instalacion" >}} "About Us").

**Correr lando con:**
```php
lando start
lando npm run dev 
```

Vite responde con:
```php
VITE v4.0.3  ready in 726 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help

  LARAVEL v9.45.1  plugin v0.7.3

  ➜  APP_URL: http://voyager.lndo.site
```

Nuestro proyecto quedara en esta carpeta:
```php
/home/enrique/laravel/lando/EnriqueSousa/voyager
```

Problema, NO renderiza elementos de tailwind
Encontré la solución aquí, agregando el watch en package.json asi:
https://laracasts.com/discuss/channels/vite/equivalent-of-vite-watch
we can then run npm run watch and have the same automatically built production resources.
If you want hot reloading as well, open a second terminal window and run npm run dev. When code changes then both scripts run, creating production assets and hot reloading.
Couple also with Freek's tip about hot reloading on blade updates https://freek.dev/2277-using-laravel-vite-to-automatically-refresh-your-browser-when-changing-a-blade-file
```php
"scripts": {
        "dev": "vite",
        "build": "vite build",
        "watch": "vite build --watch"
    }, 
```
ya no correr lando npm run dev, ahora correr:
```php
lando npm run watch 
```
Listo!
Ya puedo renderizar los elementos de tailwind!


Antes de Ejecutar el Paso 9 (Migrar las Tablas)
Vamos a instalar Voyager, ya que este nos va agregar nuevas tablas que también vamos a tener que migrar. 

### Paso 2
**Instalar Laravel Voyager**

Ir a las instrucciones de Instalación en:
https://voyager-docs.devdojo.com/getting-started/installation

Ejecutar
```php
lando composer require tcg/voyager
```

Después Instalar Voyager con datos de prueba con esta instrucción:
```php
lando php artisan voyager:install --with-dummy
```

En automático se hacen todas las migraciones no solo de las tablas iniciales que ya teníamos de laravel si no ademas tablas adicionales que agrega Voyager.

```php
   INFO  Running migrations.  

  2014_10_12_000000_create_users_table ..................................... 36ms DONE
  2014_10_12_100000_create_password_resets_table ........................... 35ms DONE
  2014_10_12_200000_add_two_factor_columns_to_users_table .................. 32ms DONE
  2016_01_01_000000_add_voyager_user_fields ................................ 35ms DONE
  2016_01_01_000000_create_data_types_table ................................ 91ms DONE
  2016_05_19_173453_create_menu_table ...................................... 83ms DONE
  2016_10_21_190000_create_roles_table ..................................... 31ms DONE
  2016_10_21_190000_create_settings_table .................................. 30ms DONE
  2016_11_30_135954_create_permission_table ................................ 29ms DONE
  2016_11_30_141208_create_permission_role_table .......................... 137ms DONE
  2016_12_26_201236_data_types__add__server_side ........................... 34ms DONE
  2017_01_13_000000_add_route_to_menu_items_table .......................... 37ms DONE
  2017_01_14_005015_create_translations_table .............................. 28ms DONE
  2017_01_15_000000_make_table_name_nullable_in_permissions_table .......... 40ms DONE
  2017_03_06_000000_add_controller_to_data_types_table .................... 196ms DONE
  2017_04_21_000000_add_order_to_data_rows_table ........................... 35ms DONE
  2017_07_05_210000_add_policyname_to_data_types_table ..................... 33ms DONE
  2017_08_05_000000_add_group_to_settings_table ............................ 31ms DONE
  2017_11_26_013050_add_user_role_relationship ............................. 76ms DONE
  2017_11_26_015000_create_user_roles_table ............................... 139ms DONE
  2018_03_11_000000_add_user_settings ...................................... 29ms DONE
  2018_03_14_000000_add_details_to_data_types_table ........................ 34ms DONE
  2018_03_16_000000_make_settings_value_nullable ........................... 32ms DONE
  2019_08_19_000000_create_failed_jobs_table ............................... 33ms DONE
  2019_12_14_000001_create_personal_access_tokens_table .................... 39ms DONE
  2022_12_31_201955_create_sessions_table .................................. 65ms DONE
```

Voyager a agregar algunas tablas que son para Post, otras tablas que son para Categorías, y también a instalado todo un sistema de roles y permisos.
También ha ejecutado algunos Seeders y también de una vez a ejecutado el comando de agregar el storage symlink a la carpeta publica, ese link simbólico apunta a  storage/app/public.

por ultimo vamos a configurar el disco donde nosotros queremos almacenar nuestras imágenes, videos u cualquier otro archivo que nosotras queramos subir a nuestro servidor, esto se configura en:
```php
config/filesystems.php
```
Como podemos ver la variable de entorno apunta ahorita a: 
'default' => env('FILESYSTEM_DISK', 'local'),

Pero la podemos cambiar a public en .env:
```php
FILESYSTEM_DISK=public
```











