+++
title = "Post: Que es JAMStack"
author = ["Matias Hernandez"]
draft = false
+++

-   tags:: <JAMstack>


## Intro {#intro}

Hola, soy Matías Hernández y esto es Cafe con Tech.

Esta semana revisamos un buzzword. Qué es JAMStack? y como comienzo a construir una app?

--

Bienvenidos a Cafe con Tech en donde te cuento de tips, tricks, noticias y otros temas del mundo del desarrollo de software durante tu coffee break. Si eres nuevo aquí, considera suscribirte.

--
Si te gusta este show, recuerda puedes apoyar su continuidad visitando matiashernandez.dev/invitameuncafe


## Que es? {#que-es}

JAMStack es un término genérico para referirse a una forma de dearrollar aplicaciones web. Es una arquitectura, modelo y filosofía que determina una forma de desarrollar una aplicación que cumple con los 5 pilares de una aplicación o framework bien "diseñado" según AWS.

-   Excelencia Operacional
-   Seguridad
-   Fiabilidad
-   Eficiencia
-   Optimización de Costo

Tradicionalmente una webapp esta compuesta de un servidor, un programa que ejecuta una serie de operacines para retornar una pagina web. Esto ocurre cientos de veces con cada visita al sigio web.

Un ejemplo de app tradicional es una app hecha con algun framework que se encarga no solo de obtener los datos, si no también de construir y servir el contenido hacia el browser. Rails, Django, Laravel, ExpressJS, etc son ejemplos de una app tradicional.


## Historia {#historia}

Pero, demos un poco de contexto que siempre es adecuado saber de donde proviene una idea.

La web comenzó de manos de Sir Tim Bernes Lee proveyendo de una forma de mostrar contenido estático por medio del uso de un lenguaje de marcado HTML. Hasta aquí todo bien, el desarrollo de sitios web fue por años simplemente para mostrar contenido.
Luego se le agregó dinamismo al utilizar lenguajes que de forma dinámica construian el contenido, como Perl y PHP y esto siguó así hasta que al rededor del año 2015 los sitios estáticos vuelven a popularizarse gracias al uso de herramientas de generación de sitios estáticos como Jekyll que permitian desarrollar rapidamente un sitio web directamente en github.
El 2016 el término JAMStack es acuñado por un grupo de desarrolladores que sintieron que "Sitio estático" no hacia correcta referencia a lo que se estaba desarrollando.
el 2016 crece la revolución de la "Web moderna" en donde se prioriza la performance, escalabilidad y experiencia de desarrollo. El término JAMStack comienza a ser adoptado por grandes grupos de desarrolladores.

-   2018 es el año de la explosión de JAMStack, herramientas como Netlify, Gatsby y Contentful crecen rápidamente popularizando aún más el término. Ese mismo año nace la JAMStack Conf.

    El resto es historia.


## Volvamos a que es {#volvamos-a-que-es}

JAMStack es un approach diferente pero a la vez conocido. Es una **app estática**. ¿Cómo?

-   Es estática en el sentido de que el contenido es servido como archivos estáticos (Markup) pero los datos utilizados pueden ser dinámicos (API)

    Por ejemplo tu ya tradicional aplicación escrita con React, cuado la pones en producción, lo que realmente tienes es un conjunto de archivos estáticos que es distribuido por algún servicio como Amazon S3 o Netlify y eso esencialmente es JAMStack.

Entonces de que se compone una app JAMStack
Por lo general son 3 componentes


### JavaScript: {#javascript}

Dynamic functionalities are handled by JavaScript. There is no restriction on which framework or library you must use.
Javascript permite proveer de dinámismo e interacción a esos archivos estáticos. Aquí es donde frameworks y librerías como React, Angular, Svelte y Vue entran en juego.

estas herramientas hacen que crear un sitio web sea mas "simple" (no más fácil en particular) ofreciendo diversas opciones para agregar dinamismo, junto con ofrecer un set de herramientas que finalmente generan un grupo de archivos estáticos que son servidor por tu CDN favorito.


### APIs: {#apis}

Server side operations are abstracted into reusable APIs and accessed over HTTPS with JavaScript. These can be third party services or your custom function.

Utilizar la versatilidad de las API es esencial para ofrecer funcionalidades a tu app JAMStack, la idea aquí es que tu aplicación puede consumir funcionalidades y datos de múltiples fuentes, algo completamente distinto al enfoque tradicional en donde los datos provenian del propio servidor.
El uso de Javascript para consumir datos sobre HTTP ha permitido el crecimiento de una miriada de servicios que proveen diversas funcionalidades, como autentificación, contenido, búsqueda, almacen de datos, etc.

Gatsby aquí hizo un gran trabajo defininiendo el concepto "Content Mesh" para describir la red de apis que se utilizan en una app JAMStack.

Al mismo tiempo gatsby fue pionero, o al menos de los primeros frameworks, en ofrecer una clara interfaz entre todas estas fuentes de datos por medio del uso de Graphql como centralizador.

Personalmente creo que la popularización de Graphql incentivo el crecimiento de JAMStack y viceversa.


### Markup: {#markup}

Websites are served as static HTML files. These can be generated from source files, such as Markdown, using a Static Site Generator.

Este componente es esencial, es finalmente la parte visible al usuario, es lo que el browser consume, aquí lo importante no es en particular como se crea, puede ser a mano o por medio de herramientas de autogeneración, si no, es importante como se distribuye.
Para ser considerado JAMStack el contenido debe ser distribuido de forma estática, lo que implica que no debe ser renderizado de forma "dinámica" por el servidor.

Si estás creando una app o sitio web y este es construido en el momento por el motor PHP (por ejemplo), no estámos hablando entonces una app JAMStack, pero si estas sirviendo un archivo HTML desde algun servicio de storage como S3, entonces suena comom JAMStack.

Una forma popular de crear el componente Markup es el uso de framework de Static Site Generators como Gatsby, Jekyll, Hugo, etc
que permiten consumir datos desde alguna o varias API durante el tiempo de "compilacion" para recrear los archivos estáticos utilizando esos datos.

Por ejemplo, piensa en que tienes una serie de posts escritos en archivos markdown hosteados en algun servicio de Cloud que ofrece una api, es posible, utilizar esa api para obtener todos los archivos que tienes  y utilizar su contenido para crear una pagina HTML para cada uno de ellos. Esto implica que finalmente lo que subiras a tu servicio de storage es una versión "pre-compilada" que será servido directamente al browser, lo que usualmente significa una experiencia de uso mucho más rápida para tus usuarios.


## Beneficios {#beneficios}

Entonces, que hace que JAMStack sea tan bueno y un término tan popular?
Primero, markting, yes. JAMStack es un término bastante marketero, muchos más que applicación esta´tica no? , pero obviamente si tiene beneficios reales como:

-   Mejor performance:
    Servir contenido previamente construido utilizando un CDN indica que ya no dependes de la capacidad de tu servidor para construir las paginas que serán servidas en donde en ocasiones de muchos usuarios (requests) el servidor sufria e incluso podia simplemente caerse y dejar sin servicio tu maravilloso e-commerce.

-   Más seguro
    No necesitas preocuparte de sobre vulnerabilidades del servidor o tu base de datos

por lo que puedes enfocar tus esfuerzos en definir permisos de acceso para la información privada utilizando alguna api de autorización como Auth0.

-   Menor Costo
    Hostear contenido estático es barato o incluso gratuito.

-   Mejor experiencia de desarrollo
    Frontend Devs se pueden enfocar en lo que mejor hacen, el desarrollo frontend, sin estar atados a una estructura monolítica. Esto usualmente sinifica un mejor y más rápido desarrollo.
-   Escalabilidad
    Si tu producto se vuelve viral de la noche a la mañana y de pronto tienes miles de usuarios activos, tu CDN lo compensará sin mayor dificultad.

El CDN te dará escalabilidad "infinita" o al menos gigantesca.


## Workflow {#workflow}

Obviamente la forma de desarrollar puede variar en cada uno pero por lo general hay un flujo de trabajo minimo ideal y bastante sencillo.

Desarrolla -> Sistema de control de versiones -> CI/CD -> Assets estáticos -> Deploy atómico -> Actualizar CDN.

Obvimanete depende de tus herramientas pero por ejemplo mi blog y todos mis otros sitios están construidos con este flow, pero quizá un poco más simplificado dado los servicios.

Desarrollo el sitio en mi editor favorito y utilizo git para guardar mis cambios y subirlos a github, todos los commit hechos a master son "escuchados" por netlify que automaticamente genera un nuevo build y directamente actualiza su CDN interno.


## Que herramientas puedo utilizar {#que-herramientas-puedo-utilizar}

Ya sabes que es JAMStack ahora la parte divertida, construir tu propia app JAMStack.
Puedes elejir un framework que te permite seleccionar casi cualquier tecnologia web.

-   Gatsby: React y Graphql (mi sitio web y el de control remoto estan hechos con Gatsby)
-   11ty Diferentees lenguajes de Template como Markdown, Handlebars, liquid, etc.
-   Hugo. Escrito en GO. Mi sitio de notas esta hecho con HUGO.
-   Nift. Escrito en C++ y Language Agnostic.
-   Scully: Angular.js
    y muchos mas que puedes ver en <https://jamstack.org/generators/>

Donde servir o almacenar tu app? Aquí hay mucho donde elejir.

-   Netlify (mi elección)
-   Vercel
-   AWS (AWS Amplify es una buena eleccion)
-   Github Pages

Que servicios o api hay disponibles?

-   Auth0 es un gran servicio de autenticación y autorizacion.
-   Cloudinary como servicio para hostear imagenes , la social card de mis sitios es generada utilizando cloudinary.
-   Sanity, Contenful o incluso Wordpress como CMS
-   Stripe o MercadoPago como plataforma de pagos

    Y Como le doy dinamismo?

Como ya hemos comentado, un sitio JAMStack provee de contenido estático en términos de que los archivos utilizados son estáticos pero su contenido puede ser tan dinámico como se requiera. Hay varias formas de ofrecer este dinamismo como también muchos servicios que utilizar

-   Puedes Utilizar React, Vue, Angular, o casi cualquier otro framework
-   Puedes utilizar funciones serverless como lo que ofrece AWS lambda, Netlify functions o Vercel
-   Utilizar bases de datos como FaunaDB
-   Formularios, comenarios, e-commerce, search, etc

En fin, aún hay mucho camino por recorrer


## Recursos {#recursos}

Dada la pasión y emoción alrededor de JAMStack existen varias comunidades online que trabajan para proveer ambientes inclusivos para permitir que todos puedan aprender sobre este modelo y como hacer de la web un mejor lugar.

La principal conferencia sobre JAMStack es la jamstackconf. La conferencia lider y "oificial" sobre esta arquitectura, organizada por Netlify.

-   Meetups
    Puedes encontrar varias y diferente meetups que buscan mantener el contenido vivo permitiendo que muchos puedan dar a conocer su trabajo en estas pequelas comunidades. Puedes encontrar una lista de estas meetups en jamstack.org/community

-   Slack
    También puedes unirte a algunas comunidades online como el Slack de JAMStack, the New Dynamic y Party Corgi Network
-   Newsletter
    Puedes unirte a algunas newsletter que te permitiran mantenerte al dia sobre el desarrollo de JAMStack
-   Guias y tutoriales
    -   En frontend masters puedes encontrar el curso de Jason Lengstorf sobre Introducción a JAMstack
    -   Egghead.io también tiene varias lecciones sobre Jamstack incluyendo videos sobre Netlify, Gatsby, Next y otros

-   Gracias especiales a Colby Fayock autor de The Jamstack Hanbook, cuyo ebook fue utilizado como parte de las referencias para este episodio. Te cuento que durante la semana de publicación de este episodio 16 de Noviembre, estaré sorteando dos copias de este buen ebook que incluye ejemplos de proyectos JAMStack.

    Podrás encontrar los links y descripciones de estos recursos en los show notes


## Salida {#salida}

Bueno, eso ha sido todo por hoy
Suscribite a Cafe con Tech en tu aplicación favorita.
Recuerda que podemos seguir la conversación en redes sociales, este show lo encuentras como @cafe\_contech en twitter e instagram y a mi como @matiasfha
No olvides que puedes suscribirte a mi newsletter visitando matiashernandez.ck.page

Y como siempre, los amigos de primate tostadores invitan el café para acompañar este episodio.
Visita primates.cafe y utiliza el código cafecontech para obtener un 10% de descuento en tu compra.

Gracias por escuchar y sige participando.
