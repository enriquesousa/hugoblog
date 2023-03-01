---
date: 2023-03-01T11:22:10-08:00
title: "Line Break ShortCode"
menuTitle: "Line Break ShortCode"
draft: false
weight: 35
---

We can use Shortcodes to break lines.

Make line_break.html file inside layouts/shortcodes and add <br /> tag to it. So line_break.html file will look like this:
```php
<br />
```

Now you can use this Shortcode to your markdown file by adding `{{< line_break >}}`. So in your case, it will look like this:
![Widgets](/hugo/line_break.png)

> A beautiful {{< line_break >}} other text or image



