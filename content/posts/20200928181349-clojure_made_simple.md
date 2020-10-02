+++
title = "Video: Clojure Made Simple"
author = ["Matias Hernandez"]
draft = false
+++

-   tags:: [Clojure]({{< relref "20200922032244-clojure" >}})[Programing Languages]({{< relref "20200927000334-programing_languages" >}})
-   source:: <https://youtu.be/VSdnJDO-xdg>
-   author:: Rich Hickey

> "A lot of the best programmer and the most productive programmers I know are writing everything in Clojure and swearing by it, and then just producing ridiculously sophisticated things in a very short time.
> And the programmer productiviy matters."
> Adrian Cockroft
> <http://thenewstack.io/the-new-stack-makers-andrian-cockcroft-on-sun-netflix-clojure-go-docker-and-more>


## Programming is an Economic Activity {#programming-is-an-economic-activity}

Que quieren los stackholders: Algo bueno y pronto.

-   Algo bueno: Va más allá de las consideraciones de "correción" del equipo de desarrollo (tests, types, etc)
    Muchas veces tests no son suficiente para definir algo bueno: que funcione desde el punto de vista de los stakeholders
-   Flexibilidad
-   Que haga lo que se supone que debe hacer: Perspectiva de los stakeholders
    -   Es dificil saber si un gran codebase hace lo que se supone que debe hacer "dificil de seguir o razonar su estado".
    -   Clojure intenta resolver esto siendo un conjunto pequeño y funcional
-   Requerimientos operacionales: Adoptar Clojure es "simple" dado que todo el proceso de "hostearlo" ya esta resuelto.
    -   Corre en la JVM - un simple jar más
    -   Corre en el browser: Un bundle de JS
    -   Lo que es verdad para la JVM o el browser lo es para Clojure (Pero a la hora de desarrollar Clojure ofrece más valor )
    -   Es suficientemente rápido en la JVM, pero en el browser tiene una gran performance utilizando[ Om]({{< relref "20200929103110-om" >}})


### How? {#how}

-   Data orientation
-   Simplicity: Opossite of complexity, not "easy"
    -   Cosas complejas son enredades, las cosas simples no.,

> What matters is not just what a programming language makes possible, but what it makes practical and idiomatic

-   Clojure se esfuerza en que "Data processing" sea idiomatico
    -   Todo app hace procesamiento de datos
    -   Data es información inumtable
    -   Los lenguajes "fiddle around". Hacen cosas que complejizan la representación de los datos (información inmutable)
    -   OOP es mas sobre el programa en si mismo en vez de ser sobre los datos
-   Clojure embraces data
    -   Data es first class
    -   Data literals
    -   Code is data
    -   La mayoría de las funciones retornan datos
    -   Información es representada como "plain data"
