---
title: "Manage Property Read All Product"
date: 2023-05-26T08:54:46-07:00
draft: false
menuTitle: "67. Manage Property Read All Product"
weight: 67
---

Ajustar en config/app.php:
```php
// 'timezone' => 'UTC',
'timezone' => 'America/Tijuana', 
```
Para tener las fechas con mi zona.

Para traducciones al español.
https://github.com/amendozaaguiar/laravel-lat-es
Puedes instalar este paquete mediante composer:
```php
composer require amendozaaguiar/laravel-lat-es
```
Ahora simplemente necesitamos actualizar las traducciones que lo haremos con el siguiente comando:
```php
php artisan vendor:publish --tag=lang
```
Esto lo podemos hacer modificando el parámetro locale de la configuración de Laravel en config/app.php:
```php
// Ej: español
'locale'          => 'es',
// Ej: inglés
'locale'          => 'en',
```
Se puede ser mas concreto e indicar las variaciones de un lenguaje:
```php
// Inglés americano
'locale' => 'en_US',
// Portugués de Portugal
'locale' => 'pt_PT',
```
No! me creo la carpeta  resources/lang/es.json

Me funciono mejor este paquete:
Laravel Espanol
https://github.com/Laraveles/spanish

Puedes instalar este paquete mediante composer:
```php
composer require laraveles/spanish
```

Ahora simplemente necesitamos actualizar las traducciones que lo haremos con el siguiente comando:
```php
php artisan vendor:publish --tag=lang
```

Esto lo podemos hacer modificando el parámetro locale de la configuración de Laravel en config/app.php:
```php
// Ej: español
'locale'          => 'es',
// Ej: inglés
'locale'          => 'en',
```

Para hacer las traducciones de los campos que inician con {{ __('Email') }} por ejemplo, en resources/views/auth/login.blade.php, entonces tenemos que crear un archivo es.json y ahi hacer las traducciones. en resources/lang/es.json:
```php
{
    "E-Mail Address": "Correo electrónico",
    "Email": "Correo",
    "Password": "Contraseña",
    "Remember Me": "Recuérdame",
    "Login": "Acceder",
    "Forgot Your Password?": "¿Olvidaste tu contraseña?",
    "Register": "Registro",
    "Name": "Nombre",
    "Confirm Password": "Confirmar contraseña",
    "Reset Password": "Restablecer contraseña",
    "Reset Password Notification": "Aviso para restablecer contraseña",
    "You are receiving this email because we received a password reset request for your account.": "Estás recibiendo este email porque se ha solicitado un cambio de contraseña para tu cuenta.",
    "This password reset link will expire in :count minutes.": "Este enlace para restablecer la contraseña caduca en :count minutos.",
    "If you did not request a password reset, no further action is required.": "Si no has solicitado un cambio de contraseña, puedes ignorar o eliminar este e-mail.",
    "Please confirm your password before continuing.": "Por favor confirme su contraseña antes de continuar.",
    "Regards": "Saludos",
    "Whoops!": "¡Ups!",
    "Hello!": "¡Hola!",
    "If you’re having trouble clicking the \":actionText\" button, copy and paste the URL below\ninto your web browser: [:actionURL](:actionURL)": "Si tienes problemas haciendo click en el botón \":actionText\", copia y pega el siguiente\nenlace en tu navegador: [:actionURL](:actionURL)",
    "If you’re having trouble clicking the \":actionText\" button, copy and paste the URL below\ninto your web browser: [:displayableActionUrl](:actionURL)": "Si tienes problemas haciendo click en el botón \":actionText\", copia y pega el siguiente\nenlace en tu navegador: [:displayableActionUrl](:actionURL)",
    "If you’re having trouble clicking the \":actionText\" button, copy and paste the URL below\ninto your web browser:": "Si tienes problemas haciendo click en el botón \":actionText\", copia y pega el siguiente\nenlace en tu navegador:",
    "Send Password Reset Link": "Enviar enlace para restablecer contraseña",
    "Logout": "Cerrar sesión",
    "Verify Email Address": "Confirmar correo electrónico",
    "Please click the button below to verify your email address.": "Por favor pulsa el siguiente botón para confirmar tu correo electrónico.",
    "If you did not create an account, no further action is required.": "Si no has creado ninguna cuenta, puedes ignorar o eliminar este e-mail.",
    "Verify Your Email Address": "Confirma tu correo electrónico",
    "A fresh verification link has been sent to your email address.": "Se ha enviado un nuevo enlace de verificación a tu correo electrónico.",
    "Before proceeding, please check your email for a verification link.": "Antes de poder continuar, por favor, confirma tu correo electrónico con el enlace que te hemos enviado.",
    "If you did not receive the email": "Si no has recibido el email",
    "click here to request another": "pulsa aquí para que te enviemos otro",
    "All rights reserved.":  "Todos los derechos reservados.",
    "Profile":  "Perfil"
}  
```

Este si funciono, traduje perfil en resources/views/admin/body/header.blade.php
```php
{{-- Profile --}}
<li class="dropdown-item py-2">
    <a href="{{ route('admin.profile') }}" class="text-body ms-0">
        <i class="me-2 icon-md" data-feather="user"></i>
        <span>{{ __('Profile') }}</span>
    </a>
</li> 
```
Listo!

Relacionar el ptype_id con la tabla property_types para desplegar nombre type_name.
En app/Models/Property.php
```php
// Relación del campo 'ptype_id' con el 'id' de la tabla 'property_types'
public function type(){
    return $this->belongsTo(PropertyType::class,'ptype_id','id');
} 
```

Ahora para desplegar el nombre en vez del 'ptype_id' 
En resources/views/backend/property/all_property.blade.php
```php
<td>{{ $item['type']['type_name'] }}</td> 
``` 
agregar nueva columna con el property_code
```php
<td>{{ $item->property_code }}</td> 
```

En la siguiente lección veremos el botón de Editar.
Listo!
