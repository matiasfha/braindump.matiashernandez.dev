+++
title = "Tipos estaticos vs tipos dinamicos"
author = ["Matias Hernandez"]
draft = false
+++

-   Tipos estático requieren que el tipo de la variable este disponible en tiempo de compilación, declarada o inferida
-   Dinámicos el tipo de la variable es conocido solo en tiempo de ejecución como Python
-   Video: <https://www.youtube.com/watch?v=C5fr0LZLMAs>

-   Type es una clasificación de informacióin: Boolean, string, number, etc
    No puedes usar un tipo para hacer algo diferente a su definición
-   Types nos permiten evitar errores inesperados al usar una variable de un tipo para hacer otra cosa.
-   Tipos estáticos: Definen el tipo en base al valor pasado.
    -   JS tiene un tipo particular \`undefined\`

-   Recordar que la compilación convierte el programa escrito en un ejecutable en compile time.
-   Algunos lenguajes revisan los tipos en tiempo de compilación ****static typing**** y otros en tiempo de ejecución **dyamic typing**
    -   C#, C++, Java, Go, Rust, Elm -> Static typing falla en compilación
    -   JS, Ruby, Python -> Ell error ocurre solo en ejecución dynamic typing
-   Dynamic: Usualmente son más fáciles de aprender, son de scripting, pero pueden generar mas errors


## Weak vs Strong typing {#weak-vs-strong-typing}

JS ejecuta coerción de tipos sin "decirte". JS es weakly typing. Tiene la noción de tipos pero no necesariamente los utiliza seriamente. Relax typing

-   Strong -> Serios

-   Es un espectro. JS is very weak.
-   JS hace implicit conversión pero hay metodos para hacerlo explicitamente.
-   Implicit -> Hidden, menos codigo pero más frágil
