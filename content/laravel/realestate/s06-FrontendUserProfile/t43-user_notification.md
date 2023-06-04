---
title: "User Login and Logout Notification"
date: 2023-05-26T08:49:28-07:00
draft: false
menuTitle: "43. User Login and Logout Notification"
weight: 43
---
Para el User Logout
En app/Http/Controllers/UserController.php
```php
// User Logout
public function UserLogout(Request $request)
{

    Auth::guard('web')->logout();
    $request->session()->invalidate();
    $request->session()->regenerateToken();

    $notification = array(
        'message' => 'Cierre de Sesión Exitosa',
        'alert-type' => 'success'
    );

    return redirect('/login')->with($notification);

} 
```
Para el Admin Logout
En app/Http/Controllers/AdminController.php
```php
// Admin Logout
public function AdminLogout(Request $request)
{

    Auth::guard('web')->logout();
    $request->session()->invalidate();
    $request->session()->regenerateToken();

    $notification = array(
        'message' => 'Cierre de Sesión Exitosa',
        'alert-type' => 'success'
    );

    return redirect('/admin/login')->with($notification);

} 
```
Y agregar el toaster drivers a resources/views/admin/admin_login.blade.php
```php
<head> 
...
{{-- Toaster cdn --}}
<link rel="stylesheet" type="text/css" href="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.css">
</head> 
<body>
...
{{-- Toaster cdn --}}
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/latest/toastr.min.js"></script>

{{-- Toaster script --}}
<script>
    @if(Session::has('message'))
        var type = "{{ Session::get('alert-type','info') }}"
            switch(type){
            case 'info':
            toastr.info(" {{ Session::get('message') }} ");
            break;

            case 'success':
            toastr.success(" {{ Session::get('message') }} ");
            break;

            case 'warning':
            toastr.warning(" {{ Session::get('message') }} ");
            break;

            case 'error':
            toastr.error(" {{ Session::get('message') }} ");
            break;
        }
    @endif
</script>
</body>
``` 

Ahora para las notificaciones del login para todos los usuarios
desplegando su nombre!
En app/Http/Controllers/Auth/AuthenticatedSessionController.php
```php
public function store(LoginRequest $request): RedirectResponse
{
    $request->authenticate();

    // Para saber quien esta haciendo el login
    $id = Auth::user()->id;
    $adminData = User::find($id);
    $userName = $adminData->name;

    $request->session()->regenerate();

    $notification = array(
        'message' => 'Usuario '. $userName .' ha iniciado sesión con éxito',
        'alert-type' => 'info'
    );

    $url = '';
    if ($request->user()->role === 'admin') {
        $url = 'admin/dashboard';
    }elseif($request->user()->role === 'agent'){
        $url = 'agent/dashboard';
    }elseif($request->user()->role === 'user'){
        $url = '/dashboard';
    }

    return redirect()->intended($url)->with($notification);
} 
```
Listo!

