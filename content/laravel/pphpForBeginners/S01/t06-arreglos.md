---
date: 2023-03-01T09:15:44-08:00
title: "Arreglos (array)"
menuTitle: "06. Arreglos (array)"
draft: false
weight: 30
---

En este punto, debes de saber cómo crear una variable para una cadena o número primitivo. Pero, ¿qué pasa con las situaciones en las que queremos declarar una colección o una lista de cosas? ¿Una lista de nombres de usuario, títulos de libros o tuits? En estas situaciones, podemos buscar arreglos.

## Que aprenderemos
- Arrays
- Foreach
- Alternative Syntax

### - Books -
### Do Androids Dream of Electric Sheep?
Info
```php
Author:	Philip K. Dick
Country: United States
Language: English
Genre: Science fiction, philosophical fiction, noir fiction
Publisher: Doubleday
Publication_date: 1968
Media_type:	Print (hardback & paperback)
Pages:	210 
wiki: https://en.wikipedia.org/wiki/Do_Androids_Dream_of_Electric_Sheep%3F
```
Description
```text
Do Androids Dream of Electric Sheep? (retrospectively titled Blade Runner: Do Androids Dream of Electric Sheep? in some later printings) is a dystopian science fiction novel by American writer Philip K. Dick, first published in 1968. The novel is set in a post-apocalyptic San Francisco, where Earth's life has been greatly damaged by a nuclear global war, leaving most animal species endangered or extinct. The main plot follows Rick Deckard, a bounty hunter who has to "retire" (i.e. kill) six escaped Nexus-6 model androids, while a secondary plot follows John Isidore, a man of sub-par IQ who aids the fugitive androids.

The book served as the basis for the 1982 film Blade Runner, even though some aspects of the novel were changed, and many elements and themes from it were used in the film's 2017 sequel Blade Runner 2049.  
```
### Four Past Midnight
Info
```php
Author:	Stephen King
Cover_artist: Rob Wood-Stansbury
Country: United States
Language: English
Genre: Supernatural fiction
Publisher: Viking
Publication_date: September 24, 1990
Media_type:	Print (hardcover)
Pages: 763
ISBN: 978-0-670-83538-6
Preceded_by: Skeleton Crew 
Followed_by: Nightmares & Dreamscapes 
```
Description
```text
Four Past Midnight is a collection of novellas written by Stephen King in 1988 and 1989 and published in August 1990. It is his second book of this type, the first one being Different Seasons. The collection won the Bram Stoker Award in 1990 for Best Collection and was nominated for a Locus Award in 1991. In the introduction, King says that, while a collection of four novellas like Different Seasons, this book is more strictly horror with elements of the supernatural. 
```
### Project Hail Mary
Info
```php
Author: Andy Weir
Audio_read_by: Ray Porter
Cover_artist: Will Staehle
Country: United States
Language: English
Genre: Science fiction
Publisher: Ballantine Books
Publication_date: May 4, 2021
Media_type: Print, ebook, audiobook
Pages: 496
Awards:	2021 Dragon Award for Best Science Fiction Novel
ISBN: 978-0-593-39556-1 
```
Description
```text
Project Hail Mary is a 2021 science fiction novel by American novelist Andy Weir. Set in the near future, it centers on junior high (middle) school-teacher-turned-astronaut Ryland Grace, who wakes up from a coma afflicted with amnesia. He gradually remembers that he was sent to the Tau Ceti solar system, 12 light-years from Earth, to find a means of reversing a solar dimming event that could cause the extinction of humanity.

It received generally positive reviews, and was a finalist for the 2022 Hugo Award for Best Novel. The unabridged audiobook is read by Ray Porter. Film rights have been purchased by Metro-Goldwyn-Mayer. Drew Goddard (who adapted The Martian, Weir's traditional publishing debut, into a 2015 film) is slated to adapt the book into a film, in which actor Ryan Gosling plans to star as Grace.
```

### Proyecto
Nuestro proyecto consiste en volver estos datos en forma dinámica, por lo pronto iniciamos desplegando solo en forma estática la información de los tres libros recomendados.
```php
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h1>Libros Recomendados</h1>
    <ul>
        <li>Do Androids Dream of Electric Sheep?</li>
        <li>Four Past Midnight</li>
        <li>Project Hail Mary</li>
    </ul>
</body>
</html> 
```  
Para empezarlo hacer dinámico tenemos ue colocar los nombres de los libros en un arreglo. Para crear un arreglo se usa la siguiente notación:
```php
$books = []; 
```
**Opciones para mostrar los libros**
Option 1 concatenando
```php
<body>
    <h1>Libros Recomendados</h1>

    <?php
        $books = [
            "Do Androids Dream of Electric Sheep?",
            "Four Past Midnight",
            "Project Hail Mary"
        ];
    ?>

    <ul>
        <?php
            foreach ($books as $book) {
                echo "<li>".$book."</li>";
            }    
        ?>
    </ul>
</body> 
```
Option 2 directo!
```php
<?php
    foreach ($books as $book) {
        echo "<li>$book</li>";
    }    
?> 
```
Como agregarle el símbolo de Trade Mark al final del titulo del libro. 
En html se pueden incluir asi.
HTML code para trademark y marca registrada:
```php
<p>RapidTables &trade;<p>
<p>RapidTables &reg;<p>
```
Los {} es para decirle a php que solo la variable $book es la que deseamos evaluar.
```php
<?php
    foreach ($books as $book) {
        echo "<li>{$book}&trade;</li>";
    }    
?> 
```
Una sintaxis alternativa para proyectos mas complejos:
```php
<?php foreach ($books as $book) :?>
    <li><?php echo $book; ?></li>
<?php endforeach; ?> 
```
Mas eficiente:
```php
<?php foreach ($books as $book) :?>
    <li><?= $book ?></li>
<?php endforeach; ?> 
```

