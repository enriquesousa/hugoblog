---
title: "LEMP stack - Install Nginx"
date: 2024-04-09T06:51:01-07:00
draft: false
menuTitle: "02. Instalar Nginx"
weight: 10
---

Instalar lemp stack en nuestro server en virtual box.
How to Run WordPress Locally (in a VirtualBox VM)
[YouTube](https://www.youtube.com/watch?v=PsMhopODLTY).
<br>

## Login al Server
`ssh enrique@localhost -p 2222`
<br>

## Update and Upgrade
`sudo apt update`
`sudo apt upgrade`
<br>

## Instalar nginx
`sudo apt install nginx`
<br>

## Check Listening Ports with netstat
[link](https://linuxize.com/post/check-listening-ports-linux/)
<br>

## How to Install and Configure Nginx on Ubuntu 20.04 | Step–by-Step Tutorial
[link](https://www.cherryservers.com/blog/how-to-install-and-configure-nginx-on-ubuntu-20-04)

`sudo ufw status`
`sudo ufw allow 80`
<br>

## Abrir un puerto para el http
Para poder visitar desde nuestro host.
Después de haber permitido al server, el trafico por el puerto 80.
Hay 2 formas de connectarnos con la virtual box.

### Network - Modo NAT
1. Agregando una regla mas para http en:
![Virtualbox-lemp_01](/virtualbox/lemp_01.png)
2. Visitar maquina desde el Host:
	1. `http://localhost:8080`

### Network - Modo Bridge
1. Si ponemos la virtual box en modo bridge, la maquina nos da un ip local en nuestra red.
2. Por ejemplo: IP: 192.168.0.6
3. Y podemos entrar a la maquina via ssh: 
4. `↪ ssh enrique@192.168.0.6`
5. Y podemos visitar la pagina via browser desde el Host: 
6. `192.168.0.6`

## Captura de Pantalla
En cualquiera de los dos casos podremos visitar la maquina y encontraremos la pagina de bienvenida de nginx.
![Virtualbox-lemp_01](/virtualbox/lemp_02.png)

Esta pagina esta muy sencilla y no necesita todavía PHP para verla. 
Despues Instalaremos PHP para poder servir correctamente una pagina web.




