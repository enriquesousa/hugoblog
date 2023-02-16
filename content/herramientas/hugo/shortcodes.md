---
title: "Shortcodes"
date: 2022-10-13T08:06:14-07:00
draft: false
weight: 8
---

### Como crear collapsible code blocks
Colocar esto en pagina de contenido.
**Nota**: Quitar los " "
```bash
"{{"< details "Código" >"}}"
    Collapsed text
"{{"< /details >"}}"
```

```php
 {{< details "Código" >}}
    Collapsed text
 {{< /details >}}
```


Y color esto en layouts/shortcodes/details.html
```php
<details>
    <div class="highlight-wrapper">
        <summary>{{ (.Get 0) | markdownify }}</summary>
        {{ .Inner | markdownify }}
    </div>
</details> 
```
