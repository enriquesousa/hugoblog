---
title: "Shortcodes"
date: 2022-10-13T08:06:14-07:00
draft: false
weight: 8
---

## Como crear collapsible code blocks
Colocar esto en pagina de contenido md.

**Nota**: Quitar los " "
```bash
"{{"< details "Código" >"}}"
    Collapsed text
"{{"< /details >"}}"
```

```text
 {{< details "Código" >}}
    Collapsed text
 {{< /details >}}
```

Y copiar esto en layouts/shortcodes/details.html
```php
<details>
    <div class="highlight-wrapper">
        <summary>{{ (.Get 0) | markdownify }}</summary>
        {{ .Inner | markdownify }}
    </div>
</details> 
```

## Ejemplo 1
{{< details "Texto" >}}
    Collapsed text
{{< /details >}}

## Ejemplo 2
{{< details "Código PHP" >}}

```php
<?php 
require 'functions.php';
require 'router.php';
```

{{< /details >}}




