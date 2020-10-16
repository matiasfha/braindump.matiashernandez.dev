+++
title = "Styled Components"
author = ["Matias Hernandez"]
draft = false
+++

-   tags:: [React]({{< relref "20200929103149-react" >}}) [React Native]({{< relref "20201001102454-react_native" >}})

-   sources: <https://flaviocopes.com/styled-components/>
    <https://www.robinwieruch.de/react-styled-components>
    <https://dev.to/christopherkade/styled-component-what-why-and-how-5gh3>

<!--listend-->

-   Styled-components es una variante o implementación de el método conocido como CSS-in-JS

-   CSS-in-JS es un approach que abstrae el modelo CSS a nivel de componente en vez de todo el documento. La idea es que CSS pueda ser "scoped" a un componente en especifico. Algunos beneficios
    -   Reduce the number of HTTP Requests
    -   Styling Fragmentation: No compatibility issues

Styled-components es una implementación de esta idea, o una API, hay otras que sigen la misma estructura como Emotion.sh
Te permite escribir CSS en tus componentes sin peocuparte por colisiones de nombre creando css "scoped" al componente.


## Why {#why}
