---
title: "Frontend Template Register Page Setup"
date: 2023-05-26T08:47:42-07:00
draft: false    
menuTitle: "36. Frontend Template Register Page Setup"
weight: 36
---
En el tab-2 Registrarse resources/views/auth/login.blade.php
```php
{{-- tab-2 Regístrate --}}
<div class="tab" id="tab-2">
    <div class="inner-box">
        <h4>Registrarse</h4>

        <form action="{{ route('register') }}" method="POST" class="default-form">
            @csrf

            {{-- Nombre --}}
            <div class="form-group">
                <label>Nombre</label>
                <input type="text" name="name" id="name" required="">
            </div>

            {{-- Email --}}
            <div class="form-group">
                <label>Correo Electrónico</label>
                <input type="email" name="email" id="email" required="">
            </div>

            {{-- Password --}}
            <div class="form-group">
                <label>Contraseña</label>
                <input type="password" name="password" id="password" required="">
            </div>

            {{-- Confirm Password --}}
            <div class="form-group">
                <label>Confirma Contraseña</label>
                <input type="password" name="password_confirmation" id="password_confirmation" required="">
            </div>

            {{-- botón regístrate --}}
            <div class="form-group message-btn">
                <button type="submit" class="theme-btn btn-one">Regístrate</button>
            </div>

        </form>

        <div class="othre-text">
            <p>Have not any account? <a href="signup.html">Register Now</a></p>
        </div>

    </div>
</div> 
``` 

Para redirigir a la pagina de login cuando visiten http://realestate.test/register
Modificar el acceso en app/Http/Controllers/Auth/RegisteredUserController.php
En vez de apuntar a return view('auth.register'); que apunte a login page!
```php
public function create(): View
{
    return view('auth.login');
} 
```
Listo!

