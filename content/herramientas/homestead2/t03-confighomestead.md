---
title: "Configuración de Homestead.yaml"
date: 2023-02-08T15:49:17-08:00
draft: false
weight: 15
---

En /home/enrique/Homestead/Homestead.yaml
```php
---
ip: "192.168.56.56"
memory: 2048
cpus: 2
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/laravel/homestead
      to: /home/vagrant/code

sites:
    # Prueba sencilla
    - map: test1.test
      to: /home/vagrant/code/test1/public

    # Probar --jet
    - map: test2.test
      to: /home/vagrant/code/test2/public

databases:
    - test1
    - test2

features:
    - mysql: true
    - mariadb: false
    - postgresql: false
    - ohmyzsh: false
    - webdriver: false

services:
    - enabled:
          - "mysql"
#    - disabled:
#        - "postgresql@11-main"

#ports:
#    - send: 33060 # MySQL/MariaDB
#      to: 3306
#    - send: 4040
#      to: 4040
#    - send: 54320 # PostgreSQL
#      to: 5432
#    - send: 8025 # Mailhog
#      to: 8025
#    - send: 9600
#      to: 9600
#    - send: 27017
#      to: 27017
 
```

