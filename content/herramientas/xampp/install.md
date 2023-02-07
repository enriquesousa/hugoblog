---
title: "MarkDown"
date: 2023-07-02T16:29:24-07:00
draft: false
weight: 5
---

### Instalar entorno de desarrollo para Laravel en Ubuntu

Primero asegurarnos de no tener instalado php, en mi caso desinstalo php8
**Uninstall php8**
```php
$ sudo apt-get purge php8.*
$ sudo apt-get autoclean
$ sudo apt-get autoremove 
```
### Instalar NodeJs y npm
Debian and Ubuntu based Linux distributions [ubuntu](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
[Node.js binary distributions](https://github.com/nodesource/distributions/blob/master/README.md) are available from NodeSource.
```php
Node.js v18.x:

Using Ubuntu
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - &&\
sudo apt-get install -y nodejs 
```

### Instalar Composer
[Download Composer Latest: v2.5.2](https://getcomposer.org/download/)
```php
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');" 
```

Most likely, you want to put the composer.phar into a directory on your PATH, so you can simply call composer from any directory (Global install), using for example:
```php
sudo mv composer.phar /usr/local/bin/composer 
```

Para poder usar composer necesitamos primero instalar PHP con xampp.

### Instalar Xampp
**¿Qué es XAMPP?**
XAMPP es el entorno más popular de desarrollo con PHP
XAMPP es una distribución de Apache completamente gratuita y fácil de instalar que contiene MariaDB, PHP y Perl. El paquete de instalación de XAMPP ha sido diseñado para ser increíblemente fácil de instalar y usar.
[xampp](https://www.apachefriends.org/es/index.html)

**Linux Preguntas frecuentes**
https://www.apachefriends.org/es/faq_linux.html

**Acceder a php y mysql desde la terminal**
Cuando instalamos xamppp se instalo php y mysql, pero tenemos que decirle al sistema de nuestra computadora donde están instalados estos programas para poder usarlos, entonces tenemos que crear unas ligas simbólicas a cada uno de estos programas.
Para PHP: 
```php
sudo ln -s /opt/lampp/bin/php /usr/bin
```
Para MYSQL: 
```php
sudo ln -s /opt/lampp/bin/mysql /usr/bin
```
 
### Permisos de Usuario
**httpd.conf**
Open /opt/lampp/etc/httpd.conf change nobody and nogroup [link](https://askubuntu.com/questions/64095/change-xampps-htdocs-web-root-folder-to-another-one)
```php
<IfModule unixd_module>
#
# If you wish httpd to run as a different user or group, you must run
# httpd as root initially and it will switch.  
#
# User/Group: The name (or #number) of the user/group to run httpd as.
# It is usually good practice to create a dedicated user and group for
# running httpd, as with most system services.
#
User nobody
Group nogroup
</IfModule> 
```

**Carpeta htdocs**
Para poder utilizar la carpeta de **/opt/lampp/htdocs** debemos priemro darle todos los permisos de acceso asi:
```php
sudo chmod -R 777 /opt/lampp/htdocs 
```

### How to create Xampp shortcut in Ubuntu's Start Menu
[Xampp shortcut](https://www.dinorunn.com/how-to-create-xampp-shortcut-in-ubuntu-start-menu/)

Recently, I try to install Xampp on Ubuntu, after installing successfully, I recognized that Xampp is not showed up on Ubuntu start menu automatically, then I worked around and found out the solution to create Xampp shortcut on Start Menu.

Firstly, cd to/usr/share/applications then create a new file with extension is *.desktop by opening the terminal then run this command: sudo touch xampp.desktop.
Open the new file with super admin right by: sudo gedit xampp.desktop
Paste following to the file content:
```php
[Desktop Entry]
Encoding=UTF-8
Name=XAMPP Control Panel
Comment=Start and Stop XAMPP
Exec=sudo /opt/lampp/manager-linux-x64.run
Icon=/opt/lampp/htdocs/favicon.ico
Categories=Application
Type=Application
Terminal=true 
```

Exec: Command to run the application (with Xampp you need sudo right).
Terminal: true if you want to open terminal when running this application. With Xampp I set value is true to type the password of sudo when running the app.

Save the file, now you have the Xampp shortcut available on start menu. Hit Windows button to check it :).

### Finalmente instalar aplicación de laravel via composer
Colocarnos en la carpeta:
```php
/opt/lampp/htdocs 
```

Instalar una nueva aplicación de Laravel via composer con:
```php
composer create-project laravel/laravel example1 
```
Para visitar la aplicación abrir navegador y entrar la url:
```php
http://localhost/example1/public/ 
```

Una segunda forma de instalar un proyecto de Laravel, es usando el instalador de [laravel](https://laravel.com/docs/8.x/installation#installation-via-composer).
```php
composer global require laravel/installer
 
laravel new example-app
cd example-app
php artisan serve 
```

### Error de acceso
Si al tratar de entrar a tu proyecto (http://localhost/example2/public/) nuevo de Laravel y te sale un erro como este:

Esto me funciono!
```php
Instalar
(https://askubuntu.com/questions/64095/change-xampps-htdocs-web-root-folder-to-another-one) 
1. sudo ./your-downloaded-xampp-file.run

2. sudo chown -hR  enrique:root /opt/lampp

3. edit your document root path to mounted windows documentroot and the following lines

<IfModule unixd_module>
   User enrique
   Group enrique
</IfModule>

LAMPP mysql can't create err file and doesn't have uninstall file (https://askubuntu.com/questions/892461/lampp-mysql-cant-create-err-file-and-doesnt-have-uninstall-file)
4. sudo chown mysql:mysql -R /opt/lampp/var/mysql  

5. Trabajar los proyectos en /opt/lampp/htdocs
```
Listo!

### Creating custom local domain
(https://ourcodeworld.com/articles/read/302/how-to-setup-a-virtual-host-locally-with-xampp-in-ubuntu)

1. Creating custom domain name instead of localhost in Ubuntu
```php
sudo micro /etc/hosts
```

2. Al final agregar:
```php
# xampp
127.0.0.5  example1.test
```

3. Ahora cambiar la configuración de Apache.
Allow the usage of custom virtual hosts:
```php
sudo micro sudo micro httpd.conf/httpd.conf
```

4. Des Comentar
```php
# Virtual hosts
Include etc/extra/httpd-vhosts.conf 
```

5. Create your first virtual host:
```php
sudo micro /opt/lampp/etc/extra/httpd-vhosts.conf
```

6. And create your own virtual host in this file. As shown in our custom domain in the vhost file of the system, the port that we are going to use is 127.0.0.5, therefore our virtual host will be:
```php
<VirtualHost 127.0.0.5:80>
  DocumentRoot "/opt/lampp/htdocs/example1/public"
  DirectoryIndex index.php

  <Directory "/opt/lampp/htdocs/example1/public">
	Options All
	AllowOverride All
	Require all granted
  </Directory>
</VirtualHost> 
```

7. Probar el Virtual Host
Apagar primero el servidor
```php
sudo /opt/lampp/lampp stop 
```
Volver a Encenderlo:
```php
sudo /opt/lampp/lampp start
```
Navegar a:
```php
http://example1.test/
```




