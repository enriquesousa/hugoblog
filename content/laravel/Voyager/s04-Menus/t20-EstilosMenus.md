---
title: "Dar Estilos a Menús"
menuTitle: "20. Dar Estilos a Menús"
date: 2022-12-31T07:47:01-08:00
draft: false
weight: 10
---

### Primero buscar una plantilla
En google podemos buscar "menu tailwind"
https://flowbite.com/docs/components/navbar/
Utilizar el Navbar with dropdown
Copy el código de este menu y crear un nuevo documento blade bajo view que se llame "my-menu.blade.php", paste ahi.
Para decirle a mi menu de Voyager que quiero utilizar esta plantilla:
```php
{{ menu('main', 'my-menu') }} 
```
2:00 
No renderea bien el VITE
voy a investigar

https://stackoverflow.com/questions/66288645/vite-does-not-build-tailwind-based-on-config

https://tailwindcss.com/docs/guides/vite

https://tailwindcss.com/docs/guides/laravel

Modifique tailwind.config.js asi:
```php
const defaultTheme = require('tailwindcss/defaultTheme');

/** @type {import('tailwindcss').Config} */
module.exports = {
    content: [
        './vendor/laravel/framework/src/Illuminate/Pagination/resources/views/*.blade.php',
        './vendor/laravel/jetstream/**/*.blade.php',
        './storage/framework/views/*.php',
        './resources/**/*.blade.php',
        './resources/**/*.js',
        './resources/**/*.vue',
    ],

    theme: {
        extend: {
            fontFamily: {
                sans: ['Nunito', ...defaultTheme.fontFamily.sans],
            },
        },
    },

    plugins: [require('@tailwindcss/forms'), require('@tailwindcss/typography')],
}; 
```

Encontré la solución aquí, agregando el watch en package.json asi:
https://laracasts.com/discuss/channels/vite/equivalent-of-vite-watch
we can then run npm run watch and have the same automatically built production resources.
If you want hot reloading as well, open a second terminal window and run npm run dev. When code changes then both scripts run, creating production assets and hot reloading.
Couple also with Freek's tip about hot reloading on blade updates https://freek.dev/2277-using-laravel-vite-to-automatically-refresh-your-browser-when-changing-a-blade-file
```php
"scripts": {
        "dev": "vite",
        "build": "vite build",
        "watch": "vite build --watch"
    }, 
```
ya no correr lando npm run dev, ahora correr:
```php
lando npm run watch 
```
Listo!
Ya pudo renderiear los elementos de tailwind!

Archivos involucrados
- resources/views/layouts/app.blade.php
```php
<!-- Scripts -->
    @vite(['resources/css/app.css', 'resources/js/app.js']) 
```    
-  package.json
```php
"watch": "vite build --watch" 
```
- tailwind.config.js
```php
content: [
        './vendor/laravel/framework/src/Illuminate/Pagination/resources/views/*.blade.php',
        './vendor/laravel/jetstream/**/*.blade.php',
        './storage/framework/views/*.php',
        './resources/**/*.blade.php',
        './resources/**/*.js',
        './resources/**/*.vue',
    ], 
```
- resources/css/app.css
```php
@tailwind base;
@tailwind components;
@tailwind utilities; 
```
- postcss.config.js
```php
module.exports = {
    plugins: {
        tailwindcss: {},
        autoprefixer: {},
    },
};
```
- vite.config.js
```php
import { defineConfig } from 'vite';
import laravel, { refreshPaths } from 'laravel-vite-plugin';
import tailwindcss from 'tailwindcss';

export default defineConfig({
    plugins: [
        laravel({
            input: [
                'resources/css/app.css',
                'resources/js/app.js',
            ],
            refresh: [
                ...refreshPaths,
                'app/Http/Livewire/**',
            ],
        }),
    ],
});
```

2:00
### Trabajar en la vista de resources/views/my-menu.blade.php
Por lo pronto vamos a quedarnos con los links solamente, uno Activo que es Home y otro inactivo.
```php
<div class="hidden w-full md:block md:w-auto" id="navbar-dropdown">
    <ul class="flex flex-col p-4 mt-4 border border-gray-100 rounded-lg bg-gray-50 md:flex-row md:space-x-8 md:mt-0 md:text-sm md:font-medium md:border-0 md:bg-white dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700">

        {{-- Home - Link Activo --}}
        <li>
            <a href="#" class="block py-2 pl-3 pr-4 text-white bg-blue-700 rounded md:bg-transparent md:text-blue-700 md:p-0 md:dark:text-white dark:bg-blue-600 md:dark:bg-transparent" aria-current="page">Home</a>
        </li>

        {{-- Link inactivo --}}
        <li>
            <a href="#" class="block py-2 pl-3 pr-4 text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">Services</a>
        </li>

        {{-- Dropdown --}}
        {{-- <li>
            <button id="dropdownNavbarLink" data-dropdown-toggle="dropdownNavbar" class="flex items-center justify-between w-full py-2 pl-3 pr-4 font-medium text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 md:w-auto dark:text-gray-400 dark:hover:text-white dark:focus:text-white dark:border-gray-700 dark:hover:bg-gray-700 md:dark:hover:bg-transparent">Dropdown <svg class="w-5 h-5 ml-1" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg></button>
            <!-- Dropdown menu -->
            <div id="dropdownNavbar" class="z-10 hidden font-normal bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700 dark:divide-gray-600">
                <ul class="py-2 text-sm text-gray-700 dark:text-gray-400" aria-labelledby="dropdownLargeButton">
                <li>
                    <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Dashboard</a>
                </li>
                <li>
                    <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Settings</a>
                </li>
                <li>
                    <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Earnings</a>
                </li>
                </ul>
                <div class="py-1">
                <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-400 dark:hover:text-white">Sign out</a>
                </div>
            </div>
        </li> --}}


    </ul>
</div> 
```

Vamos a crear un componente para decirle a las clases del tipo de botón que se va a presentar (Activo o Inactivo) es el que se va a usar. 
Para crear este componente.
resources/views/components/nav-link.blade.php:
```php
@props(['active' => false])

{{-- Si active es true asignar la clase active la que esta después de ? y si es false asignar la otra clase --}}
@php
    $classes = $active
                ? 'block py-2 pl-3 pr-4 text-white bg-blue-700 rounded md:bg-transparent md:text-blue-700 md:p-0 md:dark:text-white dark:bg-blue-600 md:dark:bg-transparent'
                : 'block py-2 pl-3 pr-4 text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent';
@endphp

{{-- Crear el enlace, en $slot es la parte que podemos modificar --}}
<a {{ $attributes->merge(['class' => $classes]) }}>
    {{ $slot }}
</a> 
```
Le pasamos una variable para decirle a este componente si el componente que queremos asignarle la clase, es un link activo o no.

Ahora ya podemos modificar resources/views/my-menu.blade.php
```php
{{-- Menu de Navbar with dropdown lo baje de https://flowbite.com/docs/components/navbar/ --}}
<nav class="bg-white border-gray-200 px-2 sm:px-4 py-2.5 rounded dark:bg-gray-900" x-data="{ open: false }">

{{-- Este primer div es para centrar todo el contenido  usando la misma clase de resources/views/navigation-menu.blade.php  --}}
<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
    {{-- Este segundo div es donde esta todo nuestro menu --}}
    <div class="flex flex-wrap items-center justify-between">
        <a href="#" class="flex items-center">
            <img src="https://flowbite.com/docs/images/logo.svg" class="h-6 mr-3 sm:h-10" alt="Flowbite Logo" />
            <span class="self-center text-xl font-semibold whitespace-nowrap dark:text-white">Flowbite</span>
        </a>
        <button @click="open = !open" type="button"
            class="inline-flex items-center p-2 ml-3 text-sm text-gray-500 rounded-lg md:hidden hover:bg-gray-100 focus:outline-none focus:ring-2 focus:ring-gray-200 dark:text-gray-400 dark:hover:bg-gray-700 dark:focus:ring-gray-600"
            aria-controls="navbar-dropdown" aria-expanded="false">
            <span class="sr-only">Open main menu</span>
            <svg class="w-6 h-6" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20"
                xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd"
                    d="M3 5a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 10a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1zM3 15a1 1 0 011-1h12a1 1 0 110 2H4a1 1 0 01-1-1z"
                    clip-rule="evenodd"></path>
            </svg>
        </button>

        <div :class="{ 'hidden': !open }" class="hidden w-full md:block md:w-auto" id="navbar-dropdown">
            <ul
                class="flex flex-col p-4 mt-4 border border-gray-100 rounded-lg bg-gray-50 md:flex-row md:space-x-8 md:mt-0 md:text-sm md:font-medium md:border-0 md:bg-white dark:bg-gray-800 md:dark:bg-gray-900 dark:border-gray-700">

                @foreach ($items as $item)
                    {{-- En caso de tener submenu en array children viene el dato   --}}
                    @if ($item->children->count())
                        {{-- Dropdown --}}
                        <li class="relative" x-data="{ open: false }">
                            <button @click="open = !open"
                                class="flex items-center justify-between w-full py-2 pl-3 pr-4 font-medium text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 md:w-auto dark:text-gray-400 dark:hover:text-white dark:focus:text-white dark:border-gray-700 dark:hover:bg-gray-700 md:dark:hover:bg-transparent">{{ $item->title }}
                                <svg class="w-5 h-5 ml-1" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20"
                                    xmlns="http://www.w3.org/2000/svg">
                                    <path fill-rule="evenodd"
                                        d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z"
                                        clip-rule="evenodd"></path>
                                </svg></button>
                            <!-- Dropdown menu -->
                            <div :class="{ 'hidden': !open }"
                                class="hidden absolute top-8 z-10 font-normal bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700 dark:divide-gray-600">

                                <ul class="py-2 text-sm text-gray-700 dark:text-gray-400"
                                    aria-labelledby="dropdownLargeButton">

                                    @foreach ($item->children as $child)
                                        <li>
                                            <a href="{{ $child->link() }}"
                                                class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">{{ $child->title }}</a>
                                        </li>
                                    @endforeach

                                </ul>

                            </div>
                        </li>
                    @else
                        {{-- Mostrar top menu items --}}
                        <li>
                            <x-nav-link :href="$item->link()" :active="request()->routeIs($item->route)">
                                {{ $item->title }}
                            </x-nav-link>
                        </li>
                    @endif
                @endforeach

                {{-- Home - Link Activo --}}
                {{-- <li>
                    <a href="#" class="block py-2 pl-3 pr-4 text-white bg-blue-700 rounded md:bg-transparent md:text-blue-700 md:p-0 md:dark:text-white dark:bg-blue-600 md:dark:bg-transparent" aria-current="page">Home</a>
                </li> --}}

                {{-- Link inactivo --}}
                {{-- <li>
                    <a href="#" class="block py-2 pl-3 pr-4 text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 dark:text-gray-400 md:dark:hover:text-white dark:hover:bg-gray-700 dark:hover:text-white md:dark:hover:bg-transparent">Services</a>
                </li> --}}

                {{-- Dropdown --}}
                {{-- <li>
                    <button id="dropdownNavbarLink" data-dropdown-toggle="dropdownNavbar" class="flex items-center justify-between w-full py-2 pl-3 pr-4 font-medium text-gray-700 rounded hover:bg-gray-100 md:hover:bg-transparent md:border-0 md:hover:text-blue-700 md:p-0 md:w-auto dark:text-gray-400 dark:hover:text-white dark:focus:text-white dark:border-gray-700 dark:hover:bg-gray-700 md:dark:hover:bg-transparent">Dropdown <svg class="w-5 h-5 ml-1" aria-hidden="true" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M5.293 7.293a1 1 0 011.414 0L10 10.586l3.293-3.293a1 1 0 111.414 1.414l-4 4a1 1 0 01-1.414 0l-4-4a1 1 0 010-1.414z" clip-rule="evenodd"></path></svg></button>
                    <!-- Dropdown menu -->
                    <div id="dropdownNavbar" class="z-10 hidden font-normal bg-white divide-y divide-gray-100 rounded-lg shadow w-44 dark:bg-gray-700 dark:divide-gray-600">
                        <ul class="py-2 text-sm text-gray-700 dark:text-gray-400" aria-labelledby="dropdownLargeButton">
                        <li>
                            <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Dashboard</a>
                        </li>
                        <li>
                            <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Settings</a>
                        </li>
                        <li>
                            <a href="#" class="block px-4 py-2 hover:bg-gray-100 dark:hover:bg-gray-600 dark:hover:text-white">Earnings</a>
                        </li>
                        </ul>
                        <div class="py-1">
                        <a href="#" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100 dark:hover:bg-gray-600 dark:text-gray-400 dark:hover:text-white">Sign out</a>
                        </div>
                    </div>
                </li> --}}

            </ul>
        </div>

    </div>
</div>

</nav> 
```
Listo!

