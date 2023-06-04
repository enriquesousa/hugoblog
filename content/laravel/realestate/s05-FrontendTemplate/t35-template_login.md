---
title: "Frontend Template Login Page Setup"
date: 2023-05-26T08:47:35-07:00
draft: false    
menuTitle: "35. Frontend Template Login Page Setup"
weight: 35
---
Tomar como base la forma de singin de:
- ~/Sites/recursos/udemy/Laravel 10 - Build Real Estate Property Listing Project A-Z/Course+Excise+Files/Course Excise Files/Frontend/signin.html

En resources/views/auth/login.blade.php
Solo la parte del tab-1 iniciar sesión
```php
{{-- tab-1 Login --}}
<div class="tab active-tab" id="tab-1">
    <div class="inner-box">
        <h4>Inicia Sesión</h4>
        <form action="{{ route('login') }}" method="post" class="default-form">
        @csrf

            {{-- Nombre --}}
            <div class="form-group">
                <label>Nombre/Correo/Teléfono</label>
                <input type="text" name="login" id="login" required="">
            </div>

            {{-- Password --}}
            <div class="form-group">
                <label>Contraseña</label>
                <input type="password" name="password" id="password" required="">
            </div>

            {{-- boton Sign In --}}
            <div class="form-group message-btn">
                <button type="submit" class="theme-btn btn-one">Iniciar</button>
            </div>

        </form>
        <div class="othre-text">
            <p>Have not any account? <a href="signup.html">Register Now</a></p>
        </div>
    </div>
</div> 
```
Listo!

