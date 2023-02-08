---
title: "Cambios en Homestead.yaml"
date: 2023-08-02T16:29:24-07:00
draft: false
weight: 20
---

Si le hacemos cambios a las propiedades de /home/enrique/Homestead/Homestead.yaml
Tenemos que volver a recargar la maquina virtual asi:
```php
vagrant reload --provision
```
Nota:
Homestead scripts are built to be as idempotent as possible. However, if you are experiencing issues while provisioning you should destroy and rebuild the machine by executing the vagrant destroy && vagrant up command.

