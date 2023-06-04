---
title: "Take Backup And Restore in Localhost"
date: 2023-05-25T17:33:25-07:00
draft: false    
menuTitle: "31. Take Backup And Restore in Localhost"
weight: 75
---
Para hacer Backup de todo el proyecto:

1. Primero remover y limpiar cache de laravel asi:
- php artisan config:cache
- php artisan cache:clear
- php artisan view:clear
- php artisan optimize

2. Comprimir toda la carpeta:
-  ~/Sites/realestate
Right click en explorer y comprimir a ZIP, queda un archivo como de 70MB:
- realestate.zip
Ahora exportar la base de datos, exportarla con phpMyadmin
- Export
- Custom
- Tick ON (IF NOT EXIST - less efficient as indexes will be generated during table creation)
- Go
Listo!

Para restaurarlo:
- pasar estos dos archivos a un carpeta y:
- extraer el realestate.zip
- crear una nueva base de datos en phpMyadmin "laravelbackup" por ejemplo.
- importar base de datos de file realestate.sql con phpMyadmin
- ahora ir a la carpeta ~/Sites/realestate_backup/realestate y correr:
- composer update
- para conectarnos a nuestra nueva base de datos importada, modificar .env
```php
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravelbackup
DB_USERNAME=enrique
DB_PASSWORD=sousa1234
```
- Probar corriendo:
- php artisan serve
Listo!

