---
title: "Layouts"
date: 2022-10-13T08:05:49-07:00
draft: false
weight: 3
---

### Que es un **layout**?
Un **layout** es cualquier tipo de página que se utiliza como **plantilla** para mostrar un contenido. Piense en ello como las partes del sitio web que permanecen consistentes en todas las páginas.

Hugo tiene convenciones estrictas para las páginas de **layout**. Echemos un vistazo a algunas de las convenciones más utilizadas.

### Algunos nombres reconocidos de layouts
Estos son algunos de los nombres de **layouts** más comunes que reconoce Hugo:

- `baseof.html` - la plantilla base (*base template*) para un sitio o sección de un sitio.
- `list.html` - plantilla usada para listar contenido en una sección de tu sitio (e.g., `/posts`)
- `single.html` - esta plantilla es utilizada para mostrar una sola pagina (e.g., `posts/first-post`)

### Nombre de las carpetas
La primera y más importante carpeta en `layouts` es `_default`. Aquí es donde viven los diseños alternativos finales para su contenido. Ejemplos típicos de páginas aquí son `_default/baseof.html` y `_default/single.html` - la plantilla base para todas las páginas y una plantilla base para todas las páginas de visualización única.

El nombre de la carpeta en la que viven sus otros diseños también es importante para Hugo. El nombre que elija será el **type** (un tema para más adelante) que Hugo le asigne y, por lo general, debe coincidir con los nombres de su contenido.

Ejemplo: 

`content/blog/my-first-post.md` → `layouts/blog/single.html`

### Anidar layouts - block y define
Tener una plantilla (*layout*) es genial!, pero si tenemos que redefinir todo el diseño cada vez que queremos cambiar una sección, será una pesadilla mantenerlo. Hugo tiene una forma simple de manejar la herencia de diseño para evitar este problema. Primero, podemos definir un `bloque` en el diseño principal:

in */layouts/_default/baseof.html*
```html
<main>
  {{ block "main" . }}
  {{ end }} 
</main>
```

A continuación, y en la plantilla hija, podemos definir cuál debe ser el bloque principal:

in */layouts/_default/list.html*
```html
{{ define "main" }}
  <h1>This will be inserted into the block</h1>
{{ end }} 
```

**Important notes:**
{{% notice info %}}
El “punto” es necesario al crear un **bloque** (no al usarlo). Esto se refiere al **contexto** actual. Explicaremos esto en detalle más adelante.
{{% /notice %}}

{{% notice info %}}
Puede tener varios bloques en una página, pero no puede anidar un bloque dentro de otro bloque.
{{% /notice %}}

### Orden de búsqueda: ¿qué significa eso?
Una característica adicional de las plantillas de Hugo es el "orden de búsqueda". Los documentos de Hugo explican esto bien en detalle, pero es mucho para consumir en este momento, así que resumamos:

**Ejemplo simple**:

Digamos que ha creado contenido como `content/blog/my-first-post.md` (una sola página) y `content/blog` (un directorio de publicaciones). Cuando construyes/sirves tu sitio, para mostrar tu contenido, Hugo revisará tus plantillas, en un orden definido:

**Desplegando una sola pagina (single page)** (`/blog/my-first-post`)

Existe una pagina una plantilla especifica “*single-page*”  (e.g., *single.html*) en *layouts/blog*?
Si no, hay una plantilla especifica “*single-page*”  en la carpeta **_default**?

**Desplegando una pagina de directorios (carpetas)** (*/blog*):

Hay una plantilla de lista “*list template*” (e.g., `list.html`) con lógica para listar el contenido de un directorio en *layouts/blog*?
Si no, hay una plantilla especifica “*list template*” en el directorio *_default*?
En cualquiera de los dos casos, si no se encuentra una plantilla adecuada, es probable que su contenido no se muestre.













