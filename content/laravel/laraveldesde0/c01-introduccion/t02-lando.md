---
title: "Usando Lando"
menuTitle: "02. Lando"
date: 2022-10-22T18:24:24-07:00
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


## Colocarnos en la carpeta donde deseamos iniciar nuestro proyecto
**Ej.**
- /home/enrique/laravel/lando/projects

No me quiere dar la version 9 de laravel, con la latest: v2.4.2, es la que usa lando!

La ultima version de composer local en mi pc:
composer_version: '2.3.10'
Con esta version si pude generar un proyecto de laravel 9.

Para crear un nuevo proyecto lo voy a usar localmente el composer.
## Nuevo proyecto de laravel 
```bash
composer create-project laravel/laravel laravel9desde0
cd laravel9desde0
```

## Crear el archivo de configuración .lando.yml
```php
name: laravel9desde0
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

## Correr en la terminal
```bash
Lando start
lando info
```

## Conectarnos a la base de datos con DBeaver
```php
Crear nueva conexión MySql:

- Server Host: localhost
- Port: 49157 (lando info)
- Database: laravel
- Username: laravel
- Password: laravel

Después renombrar la conexión en DBeaver a laravel9desde0.
```

## npm install:
```php
lando npm install
```

## Configurar la base en .env
```php
APP_URL=http://laravel9desde0.lndo.site

DB_CONNECTION=mysql
DB_HOST=database
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=laravel
```

## npm run dev:
```php
lando npm run dev
```

## Parar el servidor lando
```php
lando stop
```

## Para todos los contenedores de lando
```php
lando poweroff
```

