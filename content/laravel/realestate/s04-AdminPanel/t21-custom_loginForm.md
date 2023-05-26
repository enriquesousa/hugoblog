---
title: "Customize Login Form"
date: 2023-05-25T17:32:06-07:00
draft: false    
menuTitle: "21. Customize Login Form"
weight: 25
---

En routes/web.php:
Fuera del grupo de Admin group middleware
```php
Route::get('/admin/login', [AdminController::class, 'AdminLogin'])->name('admin.login');
```

En resources/views/admin/admin_login.blade.php:
```php
<head>
	...
	<title>Admin Login Page</title>

    <style type="text/css">
        .authlogin-side-wrapper{
            width: 100%;
            height: 100%;
            background-image: url({{ asset('upload/login.png') }});
        }
    </style>
	...
</head>

<body>
...
	{{-- Side Image --}}
    <div class="col-md-4 pe-md-0">
        <div class="authlogin-side-wrapper">

        </div>
    </div>

    {{-- Formulario de login --}}
    <div class="col-md-8 ps-md-0">
        <div class="auth-form-wrapper px-4 py-5">

            <a href="#"
                class="noble-ui-logo logo-light d-block mb-2">Fotos<span>Oficiales</span></a>
            <h5 class="text-muted fw-normal mb-4">Welcome back! Log in to your account.</h5>

            <form class="forms-sample" method="POST" action="{{ route('login') }}">
            @csrf

                {{-- Email/Name/Phone --}}
                <div class="mb-3">
                    <label for="login" class="form-label">Email/Name/Phone</label>
                    <input type="text" name="login" class="form-control" id="login"
                        placeholder="Email, Name, or Phone">
                </div>

                {{-- password --}}
                <div class="mb-3">
                    <label for="userPassword" class="form-label">Password</label>
                    <input type="password" class="form-control" id="password" name="password"
                        autocomplete="current-password" placeholder="Password">
                </div>

                {{-- Remember me --}}
                <div class="form-check mb-3">
                    <input type="checkbox" class="form-check-input" id="authCheck">
                    <label class="form-check-label" for="authCheck">
                        Remember me
                    </label>
                </div>

                {{-- botón login --}}
                <div>
                    <button type="submit" class="btn btn-outline-primary btn-icon-text mb-2 mb-md-0">
                        Login
                    </button>
                </div>

                {{-- Not a user? Sign up --}}
                <a href="register.html" class="d-block mt-3 text-muted">Not a user? Sign up</a>

            </form>

        </div>
    </div>
...
</body>
```
