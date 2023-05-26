---
title: "Multi Auth Breeze Part 3"
date: 2023-05-25T17:30:14-07:00
draft: false
menuTitle: "15. Multi Auth Breeze Part 3"
weight: 40
---

Para proteger las rutas, necesitamos usar el middleware que nos ofrece Laravel.
- php artisan make:middleware Role

Nos crea:
app/Http/Middleware/Role.php

Ahora registrar este middleware en app/Http/Kernel.php:
'role' => \App\Http\Middleware\Role::class,

Ahora trabajamos en nuestro middleware app/Http/Middleware/Role.php:
```php
public function handle(Request $request, Closure $next, $role): Response
{
    if ($request->user()->role !== $role) {
        return redirect('dashboard');
    }
    return $next($request);
}
```

Ahora para proteger las rutas aplicamos middleware de grupo en routes/web.php:
```php
// Admin group middleware
Route::middleware(['auth','role:admin'])->group(function(){
    Route::get('/admin/dashboard', [AdminController::class, 'AdminDashboard'])->name('admin.dashboard');
});

// Agent group middleware
Route::middleware(['auth','role:agent'])->group(function(){
    Route::get('/agent/dashboard', [AgentController::class, 'AgentDashboard'])->name('agent.dashboard');
});
```
Listo!