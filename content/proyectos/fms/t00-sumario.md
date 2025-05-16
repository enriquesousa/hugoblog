---
title: "Sumario"
date: 2025-05-16T10:38:51-07:00
draft: false

weight: 5
---

## Descripción General
Sistema gestor de archivos. Con este sistema (**Gestor de Archivos**) podemos gestionar subir archivos y bajar archivos a diferentes plataformas de almacenamiento de archivos (**Cloud Storage**) como lo pueden ser: AWS, VIMEO, CLOUDINARY, MAILCHIMP, y nuestro propio SERVIDOR. Todo esto controlado desde una sola aplicación. Esta aplicación NO usa Laravel, esta desarrollada solo con PHP, MySQL, Bootstrap 5 y JavaScript.  La mayoría de estas plataformas de **Cloud Storage** cuentan con un plan inicial de almacenamiento gratuito de algunos cuantos Gigas que podemos aprovechar, al sumar este tipo de almacenamiento gratuito podemos tener hasta 200 GB de archivos. 

La App usa el **SDK** de **CLOUDINARY** para almacenar en forma gratuito archivos de tipo: Imágenes, Videos, PDF, archivos ZIP etc..., con **MAILCHIMP** usamos su **SDK** para almacenar imágenes, con **PHP** y **MySQL** podemos almacenar archivos en nuestro propio servidor. Con el **SDK** de **AMAZON WEB SERVICE S3** se usa para almacenar archivos de toda clase: Imágenes, Videos, PDF, .ZIP entre otros, a costo de centavos. La App se conecta con el **SDK** de VIMEO para almacenar gratuitamente archivos de video. **SDK** significa kit de desarrollo de software. Un **SDK** es un conjunto de herramientas para crear software para una plataforma específica. Estas herramientas también permiten a los desarrolladores de aplicaciones crear una aplicación que se integre con otro programa, como un socio de medición móvil (MMP) como Adjust.

![imagen](/proyectos/fms/gestorArchivos_main_opt.png)


## Módulos funcionales 
Funcionalidades del sistema.

- Gestión de Archivos para Multiples Plataformas en la nube
- Gestor de Archivos con **AMAZON WEB SERVICE S3** 
- Gestor de Archivos con **VIMEO** 
- Gestor de Archivos con **CLOUDINARY** 
- Gestor de Archivos con **CLOUDINARY** 
- Gestor de Archivos con **MAILCHINP**
- Gestor de Archivos con **SERVIDOR Propio**
- Uso de SDK para **AMAZON S3**
- Uso de SDK para **CLOUDINARY**
- Uso de SDK para **MAILCHINP**
- Uso de PHP y MySQL para **Servidor Propio**
- Login al sistema con **Correo** y **Contraseña**
- Vista de Lista o de Grid para ver los archivos

## Algunas capturas de pantalla

![imagen](/proyectos/fms/gestorArchivos_login_opt_540x330.png)

![imagen](/proyectos/fms/gestorArchivos_main_vistaGrid_opt_540x330.png)


***
Documentación | Gestor de Archivos | fms | TJWeb

