---
title: "Admin Logout Option"
date: 2023-05-25T17:31:59-07:00
draft: false
menuTitle: "20. Admin Logout Option"
weight: 20
---

En resources/views/admin/body/header.blade.php:
```php
 <li class="dropdown-item py-2">
     <a href="{{ route('admin.logout') }}" class="text-body ms-0">
         <i class="me-2 icon-md" data-feather="log-out"></i>
         <span>Log Out</span>
     </a>
 </li>
```

En routes/web.php:
```php
Route::get('/admin/logout', [AdminController::class, 'AdminLogout'])->name('admin.logout');
```

En app/Http/Controllers/AdminController.php:
```php
use Illuminate\Support\Facades\Auth;
// Admin Logout
public function AdminLogout(Request $request){

    Auth::guard('web')->logout();
    $request->session()->invalidate();
    $request->session()->regenerateToken();
    return redirect('/login');

}
```