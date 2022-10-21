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

Para el caso de mis capítulos dentro de /home/enrique/insync/insync2022/HugoWebSites/blog/content/laravel/laraveldesde0
```bash
hugo new laravel/laraveldesde0/eloquent/_index.md
hugo new laravel/laraveldesde0/eloquent/t12-introduccion.md
```
Tenemos que usar estos comandos para poder crear nuevas paginas, NO podemos solo crear el archivo dentro de VScode.

En el contenido de _index.md
```bash
---
title: "Eloquent"
date: 2022-10-21T15:19:04-07:00
draft: false
weight: 10
---
```

En el contenido de t12-introduccion.md
```bash
---
title: "12. Laravel Eloquent Introducción"
date: 2022-10-21T15:22:44-07:00
draft: false
weight: 5
---
```
Empezar a numerar el peso desde weight: 5 y de ahi ir aumentando en incrementos de 5 a los siguientes temas.

Ejemplo de crear los sub_temas dentro de los temas eloquent y crud.
```bash
hugo new laravel/laraveldesde0/eloquent/_index.md
hugo new laravel/laraveldesde0/eloquent/t12-introduccion.md
hugo new laravel/laraveldesde0/eloquent/t13-seeders.md
hugo new laravel/laraveldesde0/eloquent/t14-factories.md
hugo new laravel/laraveldesde0/eloquent/t15-consultas.md
hugo new laravel/laraveldesde0/eloquent/t16-mutadores.md

hugo new laravel/laraveldesde0/crud/_index.md
hugo new laravel/laraveldesde0/crud/t17-listaryleer.md
hugo new laravel/laraveldesde0/crud/t18-agregaryactualizar.md
hugo new laravel/laraveldesde0/crud/t19-validar.md
hugo new laravel/laraveldesde0/crud/t20-formrequest.md
hugo new laravel/laraveldesde0/crud/t21-asignacionmasiva.md
hugo new laravel/laraveldesde0/crud/t22-eliminar.md
hugo new laravel/laraveldesde0/crud/t23-routesresource.md
``` 
De aquí en adelante solo es cuestión de modificar:
- draft: false
- weight: 5

Para ordenar los temas como los deseamos.
Por ejemplo para el caso del tema eloquent, el front matter quedaría de la siguiente manera:
**En _index.md**
```bash
---
title: "Eloquent"
date: 2022-10-21T15:19:04-07:00
draft: false
weight: 10
---
```

**En t12-introdccion.md**
```bash
---
title: "12. Laravel Eloquent Introducción"
date: 2022-10-21T15:22:44-07:00
draft: false
weight: 5
---
```

**En t13-seeders.md**
```bash
---
title: "13. Seeders en Laravel"
date: 2022-10-21T15:36:10-07:00
draft: false
weight: 10
---
```

**En t14-factories.md**
```bash
---
title: "14. Factories en Laravel"
date: 2022-10-21T15:36:21-07:00
draft: false
weight: 15
---
```

**En t15-consultas.md**
```bash
---
title: "15. Generador de consultas de eloquent"
date: 2022-10-21T15:36:28-07:00
draft: false
weight: 20
---
```

**En t16-mutadores.md**
```bash
---
title: "16. Mutadores y Accesores"
date: 2022-10-21T15:36:33-07:00
draft: false
weight: 25
---
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