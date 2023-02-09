---
title: "Correr maquina virtual homestead"
date: 2023-02-08T15:49:28-08:00
draft: false
weight: 25
---

Aseguráramos de tener creada la carpeta donde van a residir nuestros proyectos:
```php
/home/enrique/laravel/homestead 
```
Para correr la maquina y crear proyecto nuevo:
```php
cd ~/Homestead
vagrant up
vagrant ssh
cd code
laravel new test1
cd test1
```
Al correr vagrant ssh entramos a la maquina virtual Homestead y nos indica la version de Homestead.
Veo que esta se actualiza automático cuando hacemos un:
```php
vagrant reload --provision 
```


