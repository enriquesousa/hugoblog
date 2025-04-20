---
title: "Validation Form Design"
date: 2024-05-20T08:01:15-07:00
draft: false
---


## Crear una vista
Crear una vista
`En resources/views/blog/create.blade.php`
```php
@extends('layouts.master')
@section('content')

    <div class="container mt-5">
        <div class="card">

            <div class="card-header">
                <h3>Create Blog</h3>
            </div>

            <div class="card-body">
                <form>
                    <div class="mb-3">
                        <label for="exampleInputEmail1" class="form-label">Category</label>
                        <select name="category" id="" class="form-control">
                            <option value="">Tech</option>
                        </select>
                    </div>
                    
                    <div class="mb-3">
                        <label class="form-label" for="">Title</label>
                        <input type="text" class="form-control" id="">
                    </div>

                    <div class="mb-3">
                        <label class="form-label" for="">Description</label>
                        <textarea name="" id="" cols="30" rows="10" class="form-control"></textarea>
                    </div>

                    <div class="mb-3">
                        <label for="exampleInputEmail1" class="form-label">Status</label>
                        <select name="category" id="" class="form-control">
                            <option value="">Show</option>
                            <option value="">Hide</option>
                        </select>
                    </div>

                    <button type="submit" class="btn btn-primary">Submit</button>
                </form>
            </div>

        </div>
    </div>

@endsection
```

## Modificar master
Modificar master para incluir bootstrap cdn.
<details>
  <summary>Click para ver Código. </summary>
  
```
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>

    <!-- Bootstrap cdn CSS -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" integrity="sha384-T3c6CoIi6uLrA9TneNEoa7RxnatzjcDSCmG1MXxSR1GAsXEV/Dwwykc2MPK8M2HN" crossorigin="anonymous">


</head>

<body>


    @yield('content')



    <!-- Bootstrap cdn JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.min.js" integrity="sha384-BBtl+eGJRgqQAUMxJ7pMwbEyER4l1g+O15P+16Ep7Q9Q+zqX6gSbd85u4mG4QzX+" crossorigin="anonymous"></script>

</body>

</html>
```
</details>
<br>

## Mandar llamar la vista
Mandar llamar la vista
`En app/Http/Controllers/BlogController.php`
<details>
  <summary>Click para ver Código. </summary>
  
```
public function create()
{
	return view('blog.create');
}
```
</details>
<br>

## Captura de Pantalla
![captura](/laraveltips/formValidationDesign.png)
<br>


* * *
Listo.
