---
title: "Agregar Dominios hosts"
date: 2023-02-08T15:49:43-08:00
draft: false
weight: 40
---

Para entrar a modificar este archivo tenemos que usar:
```php
sudo micro /etc/hosts
```
Y agregar los sitios que ya dimos de alta en /home/enrique/Homestead/Homestead.yaml
```php
# homestead
192.168.56.56 ecommerce.test
192.168.56.56 test1.test
192.168.56.56 test2.test
```
Al hacerle cambios a /etc/hosts y los tome en cuenta el sistema operativo tenemos que:
```php
sudo service network-manager restart 
```

