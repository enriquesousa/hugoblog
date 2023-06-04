---
title: "Frontend Template Setup Part 2"
date: 2023-05-26T08:47:14-07:00
draft: false    
menuTitle: "33. Frontend Template Setup Part 2"
weight: 33
---
Segmentar en secciones.
En resources/views/frontend/frontend_dashboard.blade.php:
```php
<body>

    <div class="boxed_wrapper">

        <!-- preloader -->
        @include('frontend.home.preload')

        <!-- main header -->
        @include('frontend.home.header')

        <!-- Mobile Menu  -->
        @include('frontend.home.mobile_menu')


        {{-- Sección principal --}}
        @yield('main')

        <!-- main-footer -->
        @include('frontend.home.footer')


        <!--Scroll to top-->
        <button class="scroll-top scroll-to-target" data-target="html">
            <span class="fal fa-angle-up"></span>
        </button>

    </div>


    <!-- jequery plugins -->
    <script src="{{ asset('frontend/assets/js/jquery.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/popper.min.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/bootstrap.min.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/owl.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/wow.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/validation.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/jquery.fancybox.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/appear.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/scrollbar.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/isotope.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/jquery.nice-select.min.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/jQuery.style.switcher.min.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/jquery-ui.js') }}"></script>
    <script src="{{ asset('frontend/assets/js/nav-tool.js') }}"></script>

    <!-- main-js -->
    <script src="{{ asset('frontend/assets/js/script.js') }}"></script>

</body> 
```

En resources/views/frontend/index.blade.php
Ponemos en main content
```php
@extends('frontend.frontend_dashboard')

@section('main')
    ...
@endsection
```
Listo!

