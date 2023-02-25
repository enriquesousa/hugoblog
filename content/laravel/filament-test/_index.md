---
title: ""
menuTitle: "Filament Tutorial"
date: 2023-02-24T16:30:14-08:00
draft: false
---
![Filament-image1](/filament-test/filament-test.svg)


## Probando Filament
Instalando y probando paquete Filament php para laravel

## Resumen
Apuntes del video tutorial original [Filament Laravel TALL Admin Panel Package](https://www.youtube.com/watch?v=In8SiXqMwh0), por **Code With Tony** de Grecia!

## Acerca de esta aplicación
- Laravel 10.0.3
- PHP v8.2.0
<br>

## Entorno de Desarrollo Local
Podemos usar un entorno de desarrollo local, mas rápido!
```bash
Laravel new filamentPrueba
```
**Levantar Servidor de PHP**
```bash
php artisan serve
```
**Storage Link**
Si vamos a usar imágenes usar
```bash
php artisan storage:link
```

cambiar el *APP_URL* en .env
**
```bash
APP_URL=http://localhost:8000 
```

**Activar Vite para los usar assets css y js**
```bash
npm run dev
```

**Asignar base de datos local sqlite, en .env**
No olvidar crear archivo database/database.sqlite
```php
DB_CONNECTION=sqlite
# DB_CONNECTION=mysql
# DB_HOST=127.0.0.1
# DB_PORT=3306
# DB_DATABASE=blog
# DB_USERNAME=root
# DB_PASSWORD= 
```
**Hacer las migraciones**
```bash
php artisan migrate 
```





### Algunas imágenes del proyecto
![Filament-image1](/filament-test/filament-ss1.png)
![Filament-image1](/filament-test/filament-ss2.png)
