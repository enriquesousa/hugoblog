---
title: "User Profile Design Part 1"
date: 2023-05-26T08:48:08-07:00
draft: false    
menuTitle: "37. User Profile Design Part 1"
weight: 37
---
Setup Theme Page
Tomar:
~/Sites/recursos/udemy/Laravel 10 - Build Real Estate Property Listing Project A-Z/Course+Excise+Files/Course Excise Files/Frontend/my-profile.html

Y copiar solo estas tres secciones en resources/views/dashboard.blade.php:
```php
@extends('frontend.frontend_dashboard')
@section('main')


<!--Page Title-->
...
<!--End Page Title-->

<!-- sidebar-page-container --> 
...
<!-- sidebar-page-container -->

<!-- subscribe-section -->
...
<!-- subscribe-section end -->

@endsection
``` 
Las modificaciones las podemos ver en este commit en github
Listo!

