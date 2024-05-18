---
title: "Working With Eloquent"
date: 2024-05-17T16:01:56-07:00
draft: false
---


## Traer todos los datos.
`$blog = Blog:all()`
<br>

## Crear nuevo record.
```
$blog = new Blog();
$blog->title = 'Esto es un nuevo titulo';
$blog->body = 'Esto es un nuevo body';
$blog->status = 1;
$blog->save();
dd($blog)
```
<br>

## Update
```
$blog = Blog::find(2);
$blog->title = 'Esto es un nuevo titulo';
$blog->body = 'Esto es un nuevo body updated';
$blog->status = 1;
$blog->save();
dd($blog)
```
<br>

## Get Data Where
```
$blog = Blog::where('status', '=', 1)->get();
dd($blog);
```
<br>

## Get Data Where
```
$blog = Blog::where('status', 1)->get();
dd($blog);
```
<br>

## Get Data Where
```
$blog = Blog::where(['status' => 0, 'id' => 1])->get();
dd($blog);
```
<br>

## Delete Data
```
$blog = Blog::findOrFail(2);
$blog = delete();
dd($blog);
```
<br>



* * *
Listo!

