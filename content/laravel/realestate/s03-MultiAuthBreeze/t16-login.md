---
title: "Login With Name Email or Phone"
date: 2023-05-25T17:30:22-07:00
draft: false
menuTitle: "16. Login"
weight: 45
---

Cambiar el label del login page en resources/views/auth/login.blade.php:
```php
<!-- Email Address -->
<div>
    <x-input-label for="login" :value="__('Email/Name/Phone')" />
    <x-text-input id="login" class="block mt-1 w-full" type="text" name="login" :value="old('login')" required autofocus autocomplete="username" />
</div>
```

Ahora modificar el codigo en app/Http/Requests/Auth/LoginRequest.php:
```php
use App\Models\User;
use Illuminate\Support\Facades\Hash;

public function rules(): array
{
    return [
        'login' => ['required', 'string'],
        'password' => ['required', 'string'],
    ];
}

public function authenticate(): void
{
    $this->ensureIsNotRateLimited();

    $user = User::where('email', $this->login)
                ->orWhere('name', $this->login)
                ->orWhere('phone', $this->login)
                ->first();

    if( !$user || !Hash::check($this->password, $user->password) ){
        RateLimiter::hit($this->throttleKey());

        throw ValidationException::withMessages([
            'login' => trans('auth.failed'),
        ]);
    }

    Auth::login($user, $this->boolean('remember'));
    RateLimiter::clear($this->throttleKey());
}
```
Listo!
Ya podemos ingresas con nombre, email o phone

