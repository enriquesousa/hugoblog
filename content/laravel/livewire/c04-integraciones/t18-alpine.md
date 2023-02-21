---
title: "Comunicación entre Livewire y Alpine"
date: 2023-02-14T12:11:16-08:00
draft: false
weight: 15
menuTitle: "18. Comunicación entre Livewire y Alpine"
---

Crear un nuevo proyecto!
```bash
laravel new alpinelivewire --jet 
```

Movernos al subdirectorio
```bash
cd  alpinelivewire
```

Lando init
```bash
lando init
```

Modificar lando yml file
**.lando.yml**
```php
name: alpinelivewire
recipe: laravel
config:
  webroot: ./public
  php: '8.2'
  composer_version: 2-latest
  via: apache
  database: mysql
  cache: redis
  xdebug: false

services:

  node:
    type: node:18
    scanner: false
    ports:
      - 3009:3009
    build:
      - npm install

  mail:
    type: mailhog
    portforward: true
    hogfrom:
      - appserver

  pma:
    type: phpmyadmin
    hosts:
      - database

tooling:

  migrate:
    service: appserver
    description: php artisan migrate
    cmd: php artisan migrate
  seed:
    service: appserver
    description: php artisan migrate:fresh --seed
    cmd: php artisan migrate:fresh --seed

  npm:
    service: node
    cmd: npm

  dev:
    service: node
    cmd: npm run dev
  build:
    service: node
    cmd: npm run build

proxy:
  mail:
    - mail.alpinelivewire.lndo.site
  pma:
    - pma.alpinelivewire.lndo.site

```

Si le hacemos un **cambio** a .lando.yml
```php
lando rebuild -y 
```

**Importante:**
No olvidar modificar vite.config.js:
```php
import { defineConfig } from 'vite';
import laravel, { refreshPaths } from 'laravel-vite-plugin';

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

    server: {
        https: false,
        host: true,
        port: 3009,
        hmr: {host: 'localhost', protocol: 'ws'},
    },

}); 
```


Correr las migraciones
```bash
lando migrate 
```
 
Instalar las librerías
```bash
lando npm install 
```

Compilar css para generar el public/js/app.js que vamos a utilizar
```bash
lando dev 
```

**resources/views/layouts/app.blade.php**
Vemos que en automático ya tenemos las librerías que necesitamos.
```php
<!-- Scripts -->
@vite(['resources/css/app.css', 'resources/js/app.js'])
```


Crear el seeder para user
```bash
lando php artisan make:seeder UserSeeder 
```

**Crear User**
Crear un usuario con su password, crear:
database/seeders/UserSeeder.php
```php
<?php

namespace Database\Seeders;

use App\Models\User;
use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;

class UserSeeder extends Seeder
{
    /**
     * Run the database seeds.
     *
     * @return void
     */
    public function run()
    {
        // Crear un usuario de prueba con todo y su password
        User::create([
            'name' => 'Enrique Sousa',
            'email' => 'enrique.sousa@gmail.com',
            'password' => bcrypt('sousa1234'),
        ]);
    }
} 
```

Y mandarlo llamar en database/seeders/DatabaseSeeder.php
```php
<?php
namespace Database\Seeders;
// use Illuminate\Database\Console\Seeds\WithoutModelEvents;
use Illuminate\Database\Seeder;
class DatabaseSeeder extends Seeder
{
    public function run(): void
    {
         $this->call(UserSeeder::class);
    }
}
 
```

Y correr las migraciones con seeders
```php
lando seed 
```

Crear nuevo **componente de livewire**.
```bash
lando php artisan make:livewire Alpine 
```

**Alpine.php**
app/Http/Livewire/Alpine.php
```php
<?php
namespace App\Http\Livewire;
use Livewire\Component;
class Alpine extends Component
{
    public $count = 0;

    public function render()
    {
        return view('livewire.alpine');
    }


    public function incrementar(){
        $this->count++;
    }

}
```

**alpine.blade.php**
Y en resources/views/livewire/alpine.blade.php
```php
<div>
    {{-- Success is as dangerous as failure. --}}

    <div class="ml-8">
        <p>Count - Livewire: {{ $count }}</p>
        {{-- 'count' en x-data es la variable de Alpine y si queremos que se sincronicé con la propiedad del componente livewire '$wire.count' --}}
        {{-- tenemos que usar la propiedad @entangle --}}

        {{-- variable NO sincronizada  --}}
        {{-- <div x-data="{ count: $wire.count }">
            <p>Count dentro del Alpine <span x-text="count"></span></p>

            <br>
            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
                    @click="count++">
                Aumentar
            </button>
        </div> --}}

        {{-- <p>Count - Alpine <span x-text="count"></span></p> --}}

        <br>

        {{-- variable sincronizada  desde Alpine --}}
        <div x-data="{ count: @entangle('count') }">
            <p>Aumentar desde Alpine</p>
            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
                    x-text="count"
                    @click="count++">
                Aumentar desde Alpine
            </button>
        </div>
        <br>
        {{-- variable sincronizada  desde livewire --}}
        <div wire:click='incrementar'>
            <p>Aumentar desde Livewire</p>
            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
                    x-text="count"
                    @click="count++">
                Aumentar desde Livewire
            </button>
        </div>
        <br>
        {{-- variable sincronizada desde Alpine usando defer --}}
        <div x-data="{ count: @entangle('count').defer }">
            <p>Aumentar desde Alpine usando defer</p>
            <button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
                    x-text="count"
                    @click="count++">
                Aumentar desde Alpine usando defer
            </button>
        </div>
        {{-- Al usar defer solo modificamos la variable en el front end --}}


        {{-- Notas:
        Alpine se ejecuta del lado del cliente.
        Mientras que Livewire se ejecuta del lado del servidor --}}

    </div>


</div>
```
Notas:
Alpine se ejecuta del lado del cliente.
Mientras que Livewire se ejecuta del lado del servidor
Por eso vemos que priemro cambia la propiedad de alpine y luego cambia la propiedad de livewire, hay un pequeno retraso!

Listo!