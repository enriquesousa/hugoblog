---
title: "Valet Cpriego"
date: 2025-05-26T09:37:39-07:00
draft: false

weight: 5
---

## Descripción General
Valet Linux is a Laravel development environment for Linux minimalists. No Vagrant, no /etc/hosts file. You can even share your sites publicly using local tunnels. Yeah, we like it too.

Valet Linux configures your system to always run Nginx in the background when your machine starts. Then, using DnsMasq, Valet proxies all requests on the *.test domain to point to sites installed on your local machine.

In other words, a blazing fast Laravel development environment that uses roughly 7mb of RAM. Valet Linux isn’t a complete replacement for Vagrant or Homestead, but provides a great alternative if you want flexible basics, prefer extreme speed, or are working on a machine with a limited amount of RAM.

## Valet Cpriego
Liga: https://cpriego.github.io/valet-linux/#introduction

## Nginx
Algunos comandos para manejar a Nginx
```
sudo apt-get update
sudo apt-get install nginx
nginx -v
sudo systemctl status nginx
```

If Nginx is not running, use the following command to launch the Nginx service:
`sudo systemctl start nginx`

To set Nginx to load when the system starts, enter the following:
`sudo systemctl enable nginx`

Status de Nginx:
```
sudo systemctl status nginx
systemctl status nginx
```

# Error con mysql
Este error siempre pasa cuando uso Stacer para limpiar!

`Failed to start MySQL Community Server`

For me, I ran some cache clearing operations to free up space using tools like Stacer, and then MySQL started showing problems.

Use this to get some clues first

`grep mysql /var/log/syslog | grep ERROR`

If the error says, Could not open file '/var/log/mysql/error.log' for error logging: No such file or directory then clearly, the log files were cleaned up.

Use this to first create the MySQL directory and grant all permissions for MySQL to use that folder.

```
sudo mkdir /var/log/mysql
sudo chown -R mysql:mysql /var/log/mysql
sudo service mysql restart
```

Listo!


