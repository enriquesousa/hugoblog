---
title: "Inicio"
date: 2022-10-13T08:05:44-07:00
draft: false
weight: 2
---

### 1. Crear un proyecto nuevo

Colocarse en le carpeta que deseamos empezar a crear proyectos web de Hugo, en terminal:
```bash
hugo new site blog
```

### 2. Agregar contenido básico

En layouts, agregar index.html con algo como esto:
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Hugo | Home</title>
  </head>
  <body>
    <h1>Bienvenido a Hugo!</h1>
    <p>Hugo es el framework mas rápido del mundo para construir sitios web!</p>
  </body>
</html>
```

### 3. Servir con:
```bash
hugo server
```
o para poder ver las paginas que estan en modo borrador (*draft*)
```bash
hugo -D server
```
Ver el sitio en: 
[localhost:1313](http://localhost:1313/)


### 4. Instalar Tema **Relearn**
Install the Relearn theme by following this [documentation](https://gohugo.io/getting-started/quick-start/#step-3-add-a-theme)

This theme’s repository is: https://github.com/McShelby/hugo-theme-relearn.git

Alternatively, you can download the theme as .zip file and extract it in the themes directory

### 5. Configuración Básica
Al crear el sitio web, puede establecer un tema utilizando la opción --theme. Sin embargo, le sugerimos que modifique el archivo de configuración (`config.toml`) y establezca el tema como predeterminado. También puede agregar la sección [outputs] para habilitar la función de búsqueda.
```php
# Change the default theme to be use when building the site with Hugo
theme = "hugo-theme-relearn"

# For search functionality
[outputs]
home = [ "HTML", "RSS", "JSON"]
```

### 6. Crear tu primer Capitulo
Los capítulos son páginas que contienen otras páginas secundarias. Tiene un estilo de diseño especial y generalmente solo contiene un nombre de capítulo, el título y un breve resumen de la sección.
El tema Relearn proporciona arquetipos para crear esqueletos para su sitio web. Comience creando su primera página de capítulo con el siguiente comando:
```bash
hugo new --kind chapter basics/_index.md
```
Al abrir el archivo dado, debería ver la propiedad chapter=true en la parte superior, lo que significa que esta página es un capítulo.

De forma predeterminada, todos los capítulos y páginas se crean como borrador. Si desea representar estas páginas, elimine la propiedad draft: true de los metadatos.

### 6. Crea tu primera pagina de contenido
Luego, cree páginas de contenido dentro del capítulo creado anteriormente. Aquí hay dos formas de crear contenido en el capítulo:
```bash
hugo new basics/first-content.md
hugo new basics/second-content/_index.md
```

### 7. Construir el sitio
Cuando tu sitio esté listo para implementarse o publicar, ejecute el siguiente comando:
```bash
hugo
```
Se generará una carpeta pública que contendrá todo el contenido estático y los activos de su sitio web. Ahora se puede implementar en cualquier servidor web.

{{% notice info %}}
Este sitio web se puede publicar y alojar automáticamente con [Netlify](https://www.netlify.com/) (Lea más sobre las implementaciones automatizadas de [HUGO con Netlify](https://www.netlify.com/blog/2015/07/30/hosting-hugo-on-netlifyinsanely-fast-deploys/)). Alternativamente, puede usar las páginas de [GitHub](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
{{% /notice %}}