---
title: "Install Virtual Box"
date: 2024-04-09T06:50:23-07:00
draft: false
menuTitle: "01. Instalar VB"
weight: 5
---


En mx linux lo instale con el mx package installer.
How to Run an Ubuntu Server VM with VirtualBox (and login via SSH)  con *Tony Teaches Thec*.
[YouTube](https://www.youtube.com/watch?v=wqm_DXh0PlQ)

## Instale un server ubuntu 22.04
```
user: enrique
server: ubuntu
pasw: linuxmint
```

## Asegurarnos de Instalar
Asegurarnos de tener instalado [link](https://www.youtube.com/watch?v=n86ywwkPDMU&t=191s).
- sudo apt update
- sudo apt upgrade
- sudo apt install openssh-server
- sudo service ssh start
- sudo systemctl status ssh
- sudo ufw allow ssh
- sudo ufw enable
- sudo ufw status
<br>

## Configuración NAT
Para configurar el que podamos entrar a la maquina via `ssh`.

Entrar a la configuración a Network / Advance / Port Forwarding
![Virtualbox-image1](/virtualbox/install_01.png)

## Para entrar desde maquina host.
`ssh enrique@localhost -p 2222`

## Config Network Bridge
Es mejor dejarlo en modo network bridge.

