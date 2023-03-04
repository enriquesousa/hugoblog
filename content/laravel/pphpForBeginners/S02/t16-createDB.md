---
date: 2023-03-03T18:55:53-08:00
title: "Crear Base de Datos MySQL"
menuTitle: "16. Crear DB"
draft: false
weight: 30
---

Ahora estamos listos para comenzar a interactuar con una base de datos MySQL. Antes de escribir PHP, primero revisemos cómo conectarnos a MySQL y luego crear una base de datos y una tabla.

## MySQL Server
Asegurarnos de tener instalado MySQL server en tu maquina.

- Install and configure a [MySQL server](https://ubuntu.com/server/docs/databases-mysql)
- Connect to mysql server [without sudo](https://stackoverflow.com/questions/37239970/connect-to-mysql-server-without-sudo)

#### Connect to mysql server `without sudo`

```bash
sudo mysql -u root

CREATE USER 'enrique'@'localhost' IDENTIFIED BY '';

GRANT ALL PRIVILEGES ON *.* TO 'enrique'@'localhost' WITH GRANT OPTION;

FLUSH PRIVILEGES;
```

## Data Base manager
Para conectarnos al servidor MySQL y crear nuestra primera base de datos, necesitamos un gestor de base de datos (*Data Base manager*), en mi caso voy a usar [Dbeaver](https://dbeaver.io/), pero también podemos usar [MySQL Workbench](https://www.mysql.com/products/workbench/) o [Table Plus](https://tableplus.com/). 

## Instalación Table Plus en Linux
Ir a las descargas de [Table Plus](https://tableplus.com/blog/2019/10/tableplus-linux-installation.html).

Instalar en Ubuntu 20.04
```bash
# Add TablePlus gpg key
wget -qO - https://deb.tableplus.com/apt.tableplus.com.gpg.key | sudo apt-key add -

# Add TablePlus repo
sudo add-apt-repository "deb [arch=amd64] https://deb.tableplus.com/debian/20 tableplus main"

# Install
sudo apt update
sudo apt install tableplus
```	
![eb3156e264c5c0c9d74a2208c32caaaa.png](:/d2cc567b125d4a8ca3497fd839cfd448)


