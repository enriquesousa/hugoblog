---
title: "Frontend Template Setup Part 3"
date: 2023-05-26T08:47:27-07:00
draft: false    
menuTitle: "34. Frontend Template Setup Part 3"
weight: 34
---
Ahora seccionar el main content.
En resources/views/frontend/index.blade.php
```php
@extends('frontend.frontend_dashboard')

@section('main')

<!-- banner-section -->
@include('frontend.home.banner')

<!-- category-section -->
@include('frontend.home.category')

<!-- feature-section -->
@include('frontend.home.feature')

<!-- video-section -->
@include('frontend.home.video')

<!-- deals-section -->
@include('frontend.home.deals')

<!-- testimonial-section end -->
@include('frontend.home.testimonial')

<!-- choose us - section -->
@include('frontend.home.chooseus')

<!-- place-section -->
@include('frontend.home.place')

<!-- team-section -->
@include('frontend.home.team')

<!-- cta-section -->
@include('frontend.home.cta')

<!-- news-section -->
@include('frontend.home.news')


<!-- download-section -->
@include('frontend.home.download')

@endsection 
```
Listo!

