---
title: "Clonar"
date: 2023-02-08T15:49:52-08:00
draft: false
weight: 50
---

## Para empacar (clonar) toda la maquina virtual.
https://stackoverflow.com/questions/19094024/is-there-any-way-to-clone-a-vagrant-box-that-is-already-installed
```php
1. Package the pre-configured box => vagrant package --base preconfigured_vm --output /path/to/mybox.box
   Note that as per the docs, the --base option should be the UUID of the machine, or the name 
   VirtualBox gives the machine (found when opening the VirtualBox application).
2. Transfer the box to the computer by using scp, rsync or whatever... 
   (you also start a web server quickly by using python -m http.server PORT or ruby -run -e httpd /path/to -p PORT)
3. Init and start vagrant init preconfigured_vm /path/to/mybox.box
4. Done 
```

## Cloning a Vagrant Box on Another Machine
https://www.youtube.com/watch?v=ETBEmch7zAo

## How to Setup a Laravel Project You Cloned from Github.com
Clonar el git master y descomprimirlo

Todos los comandos desde la caja de Homestead e ir al proyecto
- vagrant ssh

Para crear vendor dir
- composer install

Para crear node_modules dir
- npm install

Creare a copy of your .env file

Generate an app encryption key
- php artisan key:generate

Create an empty database sqlite en database/
- database.sqlite

In the .env file, add database information to allow Laravel to connect to the database
- .env
https://laravel.com/docs/9.x/homestead#connecting-to-databases
Si estamos usando database de mysql
```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=test    (nombre que le dimos a la base de datos en homestead.yaml)
DB_USERNAME=homestead
DB_PASSWORD=secret
```

Storage Link
- php artisan storage:link

Migrate the database
- php artisan migrate
[Optional]: Seed the database
- php artisan migrate:fresh --seed

Compilar:
- npm run dev
o
- npm run watch


