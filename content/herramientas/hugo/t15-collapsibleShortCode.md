---
date: 2023-03-01T11:22:23-08:00
title: "Collapsible Code Block ShortCode"
menuTitle: "Collapsible ShortCode"
draft: false
weight: 40
---

## En details.html
Copiar esto en layouts/shortcodes/details.html
```php
<details>
    <div class="highlight-wrapper">
        <summary>{{ (.Get 0) | markdownify }}</summary>
        {{ .Inner | markdownify }}
    </div>
</details> 
```

## Collapsible code blocks
Mandar llamar al collapsible code blocks asi:

![Widgets](/hugo/collapsibleCode.png)


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


