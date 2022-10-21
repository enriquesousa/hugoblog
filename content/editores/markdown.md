---
title: "MarkDown"
date: 2022-10-13T08:05:44-07:00
draft: false
weight: 5
---

## Overview

This Markdown [cheat sheet](https://www.markdownguide.org/cheat-sheet/) provides a quick overview of all the Markdown syntax elements. It can’t cover every edge case, so if you need more information about any of these elements, refer to the reference guides for basic syntax and extended syntax.


## Basic Syntax
Elementos de la sintaxis básica de markdown.

| Elemento | Syntax |
| ----------- | ----------- |
| Heading        | # H1 {{< rawhtml >}} <br> {{< /rawhtml >}}## H2 {{< rawhtml >}} <br> {{< /rawhtml >}}## H3|
| Paragraph      | Text |
| Bold           |	**bold text**       |
| Italic         |	*italicized text*   |
| Blockquote	 |   > blockquote       |
| Ordered List   |   1. First item {{< rawhtml >}} <br> {{< /rawhtml >}}2. Second item {{< rawhtml >}} <br> {{< /rawhtml >}}3. Third item  |
| Unordered List |	- First item {{< rawhtml >}} <br> {{< /rawhtml >}}- Second item {{< rawhtml >}} <br> {{< /rawhtml >}}- Third item |
| Code	         | `code`   | 
| Horizontal     | Rule	--- |
| Link	         | [title](https://www.example.com) |
| Image	         | ![alt text](image.jpg) |

## Extended Syntax
Estos elementos extienden la sintaxis básica para agregar funcionalidad adicional.

| Elemento | Syntax |
| ----------- | ----------- |
| Header | \| Syntax \|  Description \| {{< rawhtml >}} <br> {{< /rawhtml >}}  \| ----------- \| ----------- \| {{< rawhtml >}} <br> {{< /rawhtml >}}\| Header \| Title \| {{< rawhtml >}} <br> {{< /rawhtml >}}\| Paragraph \| Text \| | 
| Fenced Code Block | \`\`\`{{< rawhtml >}} <br> {{< /rawhtml >}}{{{< rawhtml >}} <br> {{< /rawhtml >}}"firstName": "John",{{< rawhtml >}} <br> {{< /rawhtml >}}"lastName": "Smith",{{< rawhtml >}} <br> {{< /rawhtml >}}"age": 25{{< rawhtml >}} <br> {{< /rawhtml >}}}{{< rawhtml >}} <br> {{< /rawhtml >}}\`\`\` |
| Footnote | Here's a sentence with a footnote. \[\^1\] <br>\[\^1\]: This is the footnote. |
| Heading ID | ### My Great Heading {#custom-id} |
| Definition List | term <br>: definition |
| Strikethrough | \~\~The world is flat.\~\~ |
| Task List | \- \[x\] Write the press release <br>\- \[ \] Update the website <br>\- \[ \] Contact the media |
| Emoji [Emoji](https://www.markdownguide.org/extended-syntax/#emoji) | That is so funny! :joy: |
| Highlight | I need to highlight these ==very important words==.  |

## Using Emoji Shortcodes
Some Markdown applications allow you to insert emoji by typing emoji shortcodes. These begin and end with a colon and include the name of an emoji.

Gone camping! :tent: Be back soon.
That is so funny! :joy:

## Simple Shortcode to Insert Raw HTML in Hugo
(https://anaulin.org/blog/hugo-raw-html-shortcode/)
Sometimes, markdown is not enough – you might want to do a one-off extra-fancy thing with raw html.
Surprisingly, Hugo doesn’t seem to have a good way to let you do this in a content file. Here is how to create your own one-line custom shortcode to make that possible.

Add a shortcode template to your site, in layouts/shortcodes/rawhtml.html, with the content:
```html
<!-- raw html -->
{{.Inner}}
```

This template tells Hugo to simply render the inner content passed to your shortcode directly into the HTML of your site.
You can then use this shortcode in your markdown content, like so:
{{</*
```html
{{< rawhtml >}}
  <p class="speshal-fancy-custom">
    This is <strong>raw HTML</strong>, inside Markdown.
  </p>
{{< /rawhtml >}}
```
*/>}}

or as simple as:
{{</*
```html
{{< rawhtml >}} <br> {{< /rawhtml >}}
```
*/>}}

En el caso de desplegar esta misma tabla de **Extended Syntax** use este shortcode asi como la inserción de los caracteres especiales "|" de la siguiente forma:
{{</*
```html
| Header | \| Syntax \|  Description \| {{< rawhtml >}} <br> {{< /rawhtml >}}  \| ----------- \| ----------- \| {{< rawhtml >}} <br> {{< /rawhtml >}}\| Header \| Title \| {{< rawhtml >}} <br> {{< /rawhtml >}}\| Paragraph \| Text \| | 
| Fenced Code Block | \`\`\`{{< rawhtml >}} <br> {{< /rawhtml >}}{{{< rawhtml >}} <br> {{< /rawhtml >}}"firstName": "John",{{< rawhtml >}} <br> {{< /rawhtml >}}"lastName": "Smith",{{< rawhtml >}} <br> {{< /rawhtml >}}"age": 25{{< rawhtml >}} <br> {{< /rawhtml >}}}{{< rawhtml >}} <br> {{< /rawhtml >}}\`\`\` |
```
*/>}}

**Raw Text Block** {{< rawhtml >}} <br> {{< /rawhtml >}} 
{{</* code */>}} {{< rawhtml >}} <br> {{< /rawhtml >}}
{{</* Texto que queremos que este como comentario (raw) */>}}

## Hugo - Open External Links in a New Tab
For Hugo based websites, when a link goes to an [external website](https://digitaldrummerj.me/hugo-links-to-other-pages/#:~:text=For%20Hugo%20based%20websites%2C%20when,html.), I prefer to have them open in a new browser tab.

To accomplish the goal of opening all external links in a new tab, you need to override the Hugo default behavior for rendering links by creating the file layouts_defaults_markup\render-link.html.
In the render-link.html, we need to look at the prefix of the .Destination to see if it starts with http or https and if it does then add the `target="_blank"
Copy this code snippet into the render-link.html.
```html
<a href=“{{ .Destination | safeURL }}”{{ with .Title}} title=“{{ . }}”{{ end }}{{ if or (strings.HasPrefix .Destination “http”) (strings.HasPrefix .Destination “https”) }} target=“_blank”{{ end }} >{{ .Text | safeHTML }}</a>
```

Now when any link starts with http or https, it will open in a new tab while links within the website will open in the same tab.

Obtenido del Blog de Justin James [Categoría Hugo](https://digitaldrummerj.me/categories/hugo/).


