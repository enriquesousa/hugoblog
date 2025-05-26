---
title: "Proton Vpn"
date: 2025-05-26T09:41:34-07:00
draft: false

weight: 10
---

## Objetivo
**Proton VPN**

26-05-2025 07:05
Active Free VPN en Vivaldi

## Solución de Problema
Tuve el problema de no poder acceder a mis sitios servidos por valet (nginx)
porque tiene el dominio:
*Código:*
```php
http://phpmyadmin.test
http://portfolio.test
```

Solución con valet:
Vi la solución [aquí](https://gist.github.com/artistro08/5ab9ad7e43c000bb15c23a4f779e5449?permalink_comment_id=4729747).

Domain Setup
Typically valet uses the .test domain prefix for you to access websites on your local development environment. In order for the .test domain to work, you'll need to add the domain to your Windows hosts file under C:\Windows\System32\drivers\etc. This however is a pain and not ideal since Valet on Mac usually just works out of the box.
To fix this, We can change the domain ending in valet from .test to .localhost. Since all browsers loopback .localhost to 127.0.0.1, this resolves everything effortlessly. To do this, run the following command:
*Código:*
```php
valet domain localhost
```

Con esto ahora todos mis dominios de valet quedan así:
*Código:*
```php
http://phpmyadmin.localhost
http://portfolio.localhost
```

Con esto el Proton VPN que esta activo en vivaldi ya me permite visitar estos sitios.

Valet links
![imagen](/herramientas/valet/Pasted_valetLinks_opt.png)


***
Listo!

