---
title: "Instalación"
menuTitle: "02. Instalación"
date: 2022-12-24T07:47:01-08:00
draft: false
weight: 10
---

## Que es lando

[Lando](https://docs.lando.dev/getting-started/) es para desarrolladores que quieren:
> 
-  Rápidamente poner en marcha un ambiente de desarrollo con todas las herramientas necesarias para desarrollar todos sus proyectos.
-  Envíe estas dependencias de desarrollo local en un archivo de configuración por proyecto, el archivo de configuracion se puede compartir en tu git.
-  Automatice pasos de compilación complejos, configuraciones de prueba, implementaciones u otros flujos de trabajo repetidos más de una vez.
-  Evite el masoquismo incorporado de usar directamente **docker** o **docker-compose**.

Es un entorno de desarrollo local gratuito, de código abierto, multiplataforma y una herramienta DevOps basada en la tecnología de contenedores Docker y desarrollada por Tandem. Diseñado para trabajar con la mayoría de los principales lenguajes, marcos y servicios, Lando proporciona una manera fácil para que los desarrolladores de todos los niveles de habilidad especifiquen requisitos simples o complejos para sus proyectos y luego se pongan a trabajar rápidamente en ellos.

¡Esta es una herramienta de desarrollo!
> Tenga en cuenta que si bien puede ejecutar Lando en producción, se desaconseja, no se recomienda y 100 % no tiene soporte. ¡NO LO HAGAS!

Usar **lando** como tu herramienta de automatización y gestión de dependencias de desarrollo local.


## Instalar Lando

**Requisitos del sistema**
[Lando](https://docs.lando.dev/getting-started/installation.html#system-requirements) está diseñado para funcionar en una amplia gama de computadoras. Aquí hay algunas pautas básicas para garantizar que su experiencia con Lando sea lo más fluida posible.

**Sistema operativo**
-  macOS 10.13 o posterior
-  Windows 10 Pro+ o equivalente (por ejemplo, Windows 10 Enterprise) con Hyper-V en ejecución
- Linux con kernel versión 4.x o superior

**Requisitos del motor Docker**
Verifique también que cumple con los requisitos necesarios para ejecutar nuestro backend del motor Docker. Tenga en cuenta que el instalador de macOS y Windows Lando instalará Docker por usted si es necesario.

-  Requisitos del motor Linux Docker
-  Requisitos de Docker Desktop para Mac
-  Requisitos de Docker Desktop para Windows

**Instalar Lando**
Install package via direct download (recommended)
- Install the Docker Community Edition for your Linux version. Visit https://get.docker.com for the "quick & easy install" script. (at least version 19.03.1-ce)
Install Docker Compose.
- Download the latest .deb, .pacman or .rpm package from GitHub Double click on the package and install via your distributions "Software Center" or equivalent.
Make sure you look at the caveats below and follow them appropriately
- https://github.com/lando/lando/releases

### Paso 1
Colocarnos en la carpeta donde deseamos iniciar nuestro proyecto.

**Ejemplo**
```php
/home/enrique/laravel/lando/projects
```

Con la ultima version de composer local:
composer_version: '2.3.10'
- Para crear un nuevo proyecto lo voy a usar localmente con laravel installer.

### Asegurar que tenemos laravel installer
```php
composer global require laravel/installer
```
Ejecuta este comando si es que no tienes ya instalado laravel installer.

### Paso 2
Nuevo proyecto de laravel
Para instalar nuevo proyecto de laravel que ya incluya Jetstream
```bash
laravel new ecommerce --jet
cd ecommerce
```
Te preguntara si lo deseas instalar con:
```bash
Which Jetstream stack do you prefer?
  [0] livewire
  [1] inertia
```
Escoge livewire.

Después te preguntara:
```bash
Will your application use teams? (yes/no) [no]:
```
Escoge no. 
Esperar unos minutos a que tu proyecto sea creado!
Laravel creara tu nuevo proyecto en la carpeta "ecommerce"
Cámbiate a su carpeta:
```bash
cd ecommerce
```

### Paso 3
Crear el archivo de configuración .lando.yml
```php
name: ecommerce
recipe: laravel

config:
  php: '8.1'
  composer_version: 2-latest
  via: nginx
  webroot: public
  database: mysql:5.7
  cache: redis
  xdebug: false

services:
  node:
    type: node
   
tooling:
  npm:
    service: node
```

### Paso 4
Correr en la terminal
```bash
Lando start
```
Después de unos minutos en que ya se crearon todos loso contenedores!
```php
Your app has started up correctly.
Here are some vitals:

 NAME                  ecommerce                                          
 LOCATION              /home/enrique/laravel/lando/EnriqueSousa/ecommerce 
 SERVICES              appserver_nginx, appserver, database, cache, node  
 APPSERVER_NGINX URLS  https://localhost:32771                            
                       http://localhost:32772                             
                       http://ecommerce.lndo.site/                        
                       https://ecommerce.lndo.site/  
```
Este tiempo de espera es solo la primera vez que se van a crear los contenedores.
Como podemos observar ya podemos visitar nuestra aplicación en esas urls.
Ahora para ver mas información de nuestros contenedores.
```bash
lando info
```
La información que nos interesa para poder conectarnos a la base de datos esta en:
```php
{ service: 'database',
    urls: [],
    type: 'mysql',
    healthy: true,
    internal_connection: { host: 'database', port: '3306' },
    external_connection: { host: '127.0.0.1', port: '32770' },
    healthcheck: 'bash -c "[ -f /bitnami/mysql/.mysql_initialized ]"',
    creds: { database: 'laravel', password: 'laravel', user: 'laravel' },
    config: {},
    version: '5.7',
    meUser: 'www-data',
    hasCerts: false,
    hostnames: [ 'database.ecommerce.internal' ] }
```
El puerto que nos interesa es el external_connection.
Este external_connection puede cambiar cada vez que reiniciemos lando, con lando start, asi que hay que revisarlo y ajustarlo en las propiedades de la conexión de DBeaver cada vez que nos queramos conectar a la base de datos.

### Paso 5
Conectarnos a la base de datos con DBeaver
```php
Crear nueva conexión MySql:

- Server Host: localhost
- Port: 32770 (lando info)
- Database: laravel
- Username: laravel
- Password: laravel

Después renombrar la conexión en DBeaver a ecommerce
```
![EcommerceScreenShot](/Ecommerce/Connect-DBeaver-MySQL.png)

### Paso 6
Descargar todas las dependencias
```php
lando npm install
```

### Paso 7
Configurar la base de datos en .env
```php
APP_URL=http://ecommerce.lndo.site

DB_CONNECTION=mysql
DB_HOST=database
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=laravel
```

### Paso 8
Correr VITE
```php
lando npm run dev
```
En las nuevas versiones de laravel ya corre VITE y nos dará esto en consola:
```php
VITE v4.0.3  ready in 1112 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help

  LARAVEL v9.45.1  plugin v0.7.3

  ➜  APP_URL: http://ecommerce.lndo.site
```

**Que es VITE?**
Vite es una herramienta de construcción de interfaz moderna que proporciona un entorno de desarrollo extremadamente rápido y empaqueta su código para la producción. Al crear aplicaciones con Laravel, normalmente usará Vite para agrupar los archivos CSS y JavaScript de su aplicación en activos listos para producción.
Vite ha reemplazado a Laravel Mix en las nuevas instalaciones de Laravel.

### Paso 9
Migrar las tablas por omisión que ya trae Laravel, en database/migrations.
```php
lando php artisan migrate
```

### Paso 10
Ya podemos visitar nuestra aplicación en:
```php
http://ecommerce.lndo.site
```

![EcommerceScreenShot](/Ecommerce/laravel-inicio.png)

Para Detener a Lando!
### Parar el servidor lando
```php
lando stop
```

### Para todos los contenedores de lando
```php
lando poweroff
```

### Lista de Comandos Lando
```php
lando --help
```

### Lista los contenedores
```php
lando list
```

### Ayuda en Lista los contenedores
```php
lando list --help
```

### listo!
Con esto ya estamos listos para iniciar a crear nuestra propia aplicación PHP apoyados con el framework de Laravel.
