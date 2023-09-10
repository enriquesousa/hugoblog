---
title: "Property Insert Database Part 2"
date: 2023-05-26T08:54:23-07:00
draft: false
menuTitle: "64. Property Insert Database Part 2"
weight: 64
---

Para empezar a traer los datos a la tabla 'properties', en nuestro método StoreProperty(Request $request)
En app/Http/Controllers/Backend/PropertyController.php
```php
public function StoreProperty(Request $request){

    $amen = $request->amenities_id;
    dd($amen);
}
```
Para checcar que nos llega con el campo "comodidades" donde colocamos codigo para 
recuperar multiples comodidades.
Vamos viendo que nos entrega.
```php
array:3 [▼ // app/Http/Controllers/Backend/PropertyController.php:39
  0 => "2"
  1 => "6"
  2 => "7"
] 
```
Nos regresa un array con lso id de los multiples items que escogimos.
Para poder separar estos items, tenemos que usar la funcion implode() asi:
```php
$amen = $request->amenities_id;
// dd($amen);
$comodidades = implode(",", $amen);
dd($comodidades); 
```
Esto nos da:
```php
"2,6,7" // app/Http/Controllers/Backend/PropertyController.php:41 
```
Como podemos ver nos entrega un string con los datos separados por coma.
Este tipo de string es el que nos conviene guardar en el campo 'amenities_id' de la tabla 'properties'.

En el campo de 'property_code' tiene que ser unique y tiene que ser automaticamente generado.
Para eso podemos usar un paquete de laravel "Laravel ID Generator":
```php
composer require haruncpi/laravel-id-generator
```
Para usar el paquete:
```php
use Haruncpi\LaravelIdGenerator\IdGenerator;
```
Para generar el codigo unique
```php
$pcode = IdGenerator::generate([
            'table' => 'properties',
            'field' => 'property_code',
            'length' => 5, 
            'prefix' => 'PC' 
        ]); 
```
 Asi queda en app/Http/Controllers/Backend/PropertyController.php
 ```php
 ...
use Intervention\Image\Facades\Image;
use Haruncpi\LaravelIdGenerator\IdGenerator;
use Carbon\Carbon;
 ...
// Store Property
public function StoreProperty(Request $request){

    $amen = $request->amenities_id;
    // dd($amen);
    $comodidades_str = implode(",", $amen);
    // dd($comodidades);

    // Generar un código unique de 5 dígitos con un prefix property code (PC)
    $pcode = IdGenerator::generate([
                    'table' => 'properties',
                    'field' => 'property_code',
                    'length' => 5,
                    'prefix' => 'PC'
                ]);

    $image = $request->file('property_thambnail');
    $name_gen = hexdec(uniqid()).'.'.$image->getClientOriginalExtension(); // crear un unique id para la imagen
    Image::make($image)->resize(370,250)->save('upload/property/thambnail/'.$name_gen);
    $save_url = 'upload/property/thambnail/'.$name_gen;

    $property_id = Property::insertGetId([

        'ptype_id' => $request->ptype_id,
        'amenities_id' => $comodidades_str,
        'property_name' => $request->property_name,
        'property_slug' => strtolower(str_replace(' ', '-', $request->property_name)),
        'property_code' => $pcode,
        'property_status' => $request->property_status,

        'lowest_price' => $request->lowest_price,
        'max_price' => $request->max_price,
        'short_descp' => $request->short_descp,
        'long_descp' => $request->long_descp,
        'bedrooms' => $request->bedrooms,
        'bathrooms' => $request->bathrooms,
        'garage' => $request->garage,
        'garage_size' => $request->garage_size,

        'property_size' => $request->property_size,
        'property_video' => $request->property_video,
        'address' => $request->address,
        'city' => $request->city,
        'state' => $request->state,
        'postal_code' => $request->postal_code,

        'neighborhood' => $request->neighborhood,
        'latitude' => $request->latitude,
        'longitude' => $request->longitude,
        'featured' => $request->featured,
        'hot' => $request->hot,
        'agent_id' => $request->agent_id,
        'status' => 1,
        'property_thambnail' => $save_url,
        'created_at' => Carbon::now(),

    ]);

}  
 ```
Listo!