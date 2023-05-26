---
title: "Admin Template Setup"
date: 2023-05-25T17:31:20-07:00
draft: false
menuTitle: "18. Admin Template Setup"
weight: 10
---

Copiar todo el folder de assets:
/home/enrique/Sites/recursos/udemy/Laravel 10 - Build Real Estate Property Listing Project A-Z/Course+Excise+Files/Course Excise Files/Backend Theme/assets
a:
/home/enrique/Sites/realestate/public

Ahora copiar las carpetas pages y partials de:
/home/enrique/Sites/recursos/udemy/Laravel 10 - Build Real Estate Property Listing Project A-Z/Course+Excise+Files/Course Excise Files/Backend Theme/Main
a:
/home/enrique/Sites/realestate/public/backend

Copiar el dashboard.html de:
/home/enrique/Sites/recursos/udemy/Laravel 10 - Build Real Estate Property Listing Project A-Z/Course+Excise+Files/Course Excise Files/Backend Theme/Main
a:
resources/views/admin/admin_dashboard.blade.php

Actualizar los assets con:
{{asset('') }}

Para parcializar nuestro contenido creamos:
resources/views/admin/index.blade.php
```php
@extends('admin.admin_dashboard')
@section('admin')

    {{-- Contenido --}}
    <div class="page-content"> 
    	...
    </div>
    <!-- End Contenido -->

@endsection
```

En resources/views/admin/admin_dashboard.blade.php:
```php
<!DOCTYPE html>

<html lang="en">

<head> 
	...
</head>

<body>

	<div class="main-wrapper">
	
		<!-- partial:partials/_sidebar.html -->
	    <nav class="sidebar">
			...
		</nav>
	    <!-- partial -->
	
	    <div class="page-wrapper">
	    
			<!-- Header Top Bar partial:partials/_navbar.html -->
          	<nav class="navbar">    
				...
			</nav>

			{{-- Contenido --}}
	      	@yield('admin')

			<!-- partial:partials/_footer.html -->
	      	<footer>
	      		...
      		</footer>
	   		 
	     </div>
	</div>
	
    <!-- core:js -->
    <script src="{{ asset('../assets/vendors/core/core.js') }}"></script>
    <!-- endinject -->
    			
	...
	
</body>

</html>
```
