---
title: "Otros"
date: 2022-10-13T08:06:18-07:00
draft: false
weight: 15
---

## Archivos
hugo new hugo/install.md
hugo new hugo/inicio.md
hugo new hugo/layouts.md
hugo new hugo/templating.md
hugo new hugo/partials.md
hugo new hugo/content.md
hugo new hugo/taxonomies.md
hugo new hugo/shortcodes.md
hugo new hugo/data.md

## Links
- How to Create a Simple, [Free Blog](https://chrisjhart.com/Creating-A-Simple-Free-Blog-Hugo/#create-github-repository) with Hugo and GitHub Pages
- Hugo 101 By [Cloudcannon](https://cloudcannon.com/community/learn/hugo-tutorial/)

## InfoBox
{{% notice info %}}
{{< rawhtml >}} <i class="fas fa-info"></i> {{< /rawhtml >}}
 Bookmark this page and the [official Commonmark reference](https://commonmark.org/help/) for easy future reference!
{{% /notice %}}

## Inline FontAwesome SVGs in Hugo ([Link](https://me.micahrl.com/blog/inline-fontawesome-svg-hugo/))
I have recently started using icons from FontAwesome, and I found an excellent post that shows how you can inline FontAwesome SVGs. This pulls the SVG data into the HTML of each page, so that a single GET request receives the page and any icons.