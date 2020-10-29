+++
title = "Clojure server con http-kit"
author = ["Matias Hernandez"]
draft = false
+++

-   tags::[Clojure]({{< relref "20200922032244-clojure" >}})

Crearemos un pequeño servidor web utilizando dos librerías bastante populares http-kit que provee de las funcionalidades de servidor y compojure
para el manejo de rutas

Utilizaremos Leiningen como herramienta para crear y manejar el proyecto. Esta es la herramienta más usada casi estandard.

Para crear la aplicacion, en la terminal iniciamos leiningen

\`lein new app server\`

Una vez finalizado el proceso crear algunos archivos dentro del directorio server

Primero agregaremos nuestras dependencias en el archivo project.clj

Agregamos \`http-kit "2.3.0"\` y \`compojure "1.6.1"\`

Ahora nos enfocamos en el codigo.
Este se encuentra definido en el archivo core.clj

Lo primero es requerir o importar la libreria. Le añadimos el alias  \`server\`

Definimos la funcion prinicipal llamada \`-main\`
Primero definimos una variable para el puerto que utilizará un valor obtenido desde el sistema o por defecto "8080"
Luego iniciamos el servidor utilizando \`run-server\` que recibe una referencia a una funcion que debemos definir y el puerto
e imprimimos en pantalla que el server esta funcionando

Ahora definimos la funcion que llamaremos baby-steps, recibe un argumento llamado req y simplemente retorna un map con status 200, headers text/html y como body on texto "Hola Mundo"

Ejecutamos con lein run y podemos visitiar en el browser y ver el resultado.

Ahora agregaremos algo de routing.

Primero importamos la lirbreria compojure y sus namespaces core y route

crearemos una funciona para definir nuestras rutas

\`app-routes\`
y usaremos las macros definidas por compojure para definir cada ruta
Una ruta GET en el root del sitio que ejecutara la funcion get-handler
Una ruta POST en /post que ejecutará la funcipon post-handler
y un error 404

ahora definimos la funcion get-handler, que será identica a nuestra baby-steps original
Seguimos con la funcion post-handler para manejar peticiones POST

que tambien recibe un argumento y retorna un nuevo map con una respuesta por defecto y estado 200
Ahora ejecutamos \`lein run\` en la terminal, abrimos el navegador y podemos probar nuestro server
Para el método post podemos probarlo con devtools y escribir en la consola una llamada fetch tipo post y ver lo que retorna.

En resumen, Clojure ofrece suficientes herramientas y un poderoso sistema de macros para crear rápidamente un servidor web con diferentes rutas para implementar un sistema CRUD
