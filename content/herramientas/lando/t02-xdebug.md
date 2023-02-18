---
title: "Xdebug"
date: 2023-02-17T17:47:49-08:00
draft: false
weight: 10
---

## Using Lando with VSCode
[link](https://docs.lando.dev/guides/lando-with-vscode.html)

Visual Studio Code is a great open source editor for programming. Debugging PHP applications with it can be easy too.

This is a basic setup to help you in this task.

Enable Xdebug by adding some lines to your Lando recipe.
```php
name: mywebsite
recipe: drupal9
services:
  appserver:
    webroot: web
    xdebug: debug
    config:
      php: .vscode/php.ini 
```




## Mi config
```php
name: datatable
recipe: laravel
config:
  webroot: ./public
  php: '8.1'
  composer_version: 2-latest
  via: apache
  database: mysql
  cache: redis
  xdebug: true

services:
  appserver:
    webroot: ./public
    xdebug: debug
    config:
      php: .vscode/php.ini
    overrides:
      environment:
        XDEBUG_MODE:

  node:
    type: node:18
    scanner: false
    ports:
      - 3009:3009
    build:
      - npm install

  # mail:
  #   type: mailhog
  #   portforward: true
  #   hogfrom:
  #     - appserver

  # pma:
  #   type: phpmyadmin
  #   hosts:
  #     - database

tooling:

  xdebug-on:
    service: appserver
    description: Enable xdebug for Apache.
    cmd: rm -f /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && docker-php-ext-enable xdebug && /etc/init.d/apache2 reload && echo "Xdebug enabled"
    user: root

  xdebug-off:
    service: appserver
    description: Disable xdebug for Apache.
    cmd: rm -f /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini && /etc/init.d/apache2 reload && echo "Xdebug disabled"
    user: root

  migrate:
    service: appserver
    cmd: php artisan migrate
  npm:
    service: node
    cmd: npm
  dev:
    service: node
    cmd: npm run dev
  build:
    service: node
    cmd: npm run build

# proxy:
#   mail:
#     - mail.datatable.lndo.site
  # pma:
  #   - pma.datatable.site 
```