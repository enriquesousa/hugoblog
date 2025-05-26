---
title: "Valet"
date: 2025-05-26T09:34:55-07:00
draft: false
---

## Descripción General
Valet Linux is a Laravel development environment for Linux minimalists. No Vagrant, no /etc/hosts file. You can even share your sites publicly using local tunnels. Yeah, we like it too.

Valet Linux configures your system to always run Nginx in the background when your machine starts. Then, using DnsMasq, Valet proxies all requests on the *.test domain to point to sites installed on your local machine.

In other words, a blazing fast Laravel development environment that uses roughly 7mb of RAM. Valet Linux isn’t a complete replacement for Vagrant or Homestead, but provides a great alternative if you want flexible basics, prefer extreme speed, or are working on a machine with a limited amount of RAM.


### Lista de Temas
{{% children  %}}