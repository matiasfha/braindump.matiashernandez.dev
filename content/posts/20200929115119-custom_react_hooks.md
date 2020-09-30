+++
title = "Custom React Hooks"
author = ["Matias Hernandez"]
draft = false
+++

tags
: [React]({{< relref "20200929103149-react" >}}) [Hooks]({{< relref "20200929115134-hooks" >}})

La api de React hook fue una gran e innovadora adición que permite una mejor organización de gran parte de la lógica de negocio que escribimos a diario en una app React.
React ofrece algunos hooks por defecto, pero el verdedor poder de esta API es la posibilidad de crear hooks personalizados que podemos compartir tanto dentro de nuestra propia aplicación como también como paquetes individuales en npm.


## [¿Qué son los hooks?](20200929115134-hooks.md) {#qué-son-los-hooks--20200929115134-hooks-dot-md}


## Custom hooks {#custom-hooks}

¿ Por qué crear mis propios hooks?

-   Lógica repetida y redudante en múltiples componentes
-   Imposible encontrar una abstracción simple que permita compartir esta lógica (HOC, render props)

Crear hooks personalizados te permite extender las funcionalidades de los hooks base además obtienes algunos otros beneficios como

-   Compartir
    Un hook, es una función, que recibe algún parámetro y retorna algún dato, lo que implica que compartir la lógica creada con una colección de hooks es simplemente extraer y crear una función que contenga el proceso. Siendo una función, utilizarla en otros componentes se sencillo y directo lo que nos permite compartir lógica de una forma funcional en nuestra aplicación.
-   Componentes más simples
    El uso de hooks reduce drásticamente (siempre y cuando se lleven a cabo buenas prácticas de desacople) el tamaño (en lineas de código) de un componente lo que simplifica el código, mejorando así la matenibilidad del mismo.


## Entonces ¿que son los hooks personalizados? {#entonces-que-son-los-hooks-personalizados}

Un hook personalizado es solo una función que usa otros hooks (incluso otros hooks personalizados). Por convención ( y para cumplir con la regla de los hooks ), el nombre del hook debe comenzar con el prefijo \`use\`

Digamos que existen dos componentes (funciones) que implementan alguna lógica comun. Es posible extraer esta lógica común a una tercera función y simplemente utilizarla dentro de las dos funciones originales. Esto es algo que se hace cada día, solo que ahora podemos hacerlo con lógica de un componente.

Hay mucho casos de uso que probablemente ya han sido resueltos por la comunidad JS que ha puesto la solución disponible para todos en un paquete en npm. Esto es otra ventaja de crear hooks personalizados. Puesto que son funciones es posible empaquetar esta lógica y compartirla con el mundo.


## Veamos un ejemplo {#veamos-un-ejemplo}

<https://egghead.io/lessons/react-crear-una-api-flexible-utilizando-react-hooks-personalizados?pl=hooks-3d62?af=4cexzz>

Podemos revisar otro ejemplo, más sencillo pero no menos interesante.
Digamos que quieres cambiar el titulo de tu documento HTML con algún valor del estado de un componente, para esto necesitas hacer uso del hook \`useEffect\` que - **\* recuerda: no es un reemplazo para los métodos del ciclo de vida usados en las clases \*** - sincroniza el estado interno del componente con algún estado externo, es decir, ejecuta algún efecto.
El código para este componente podría ser algo asi:

{{< highlight jsx "linenos=table, linenostart=1" >}}
const PostPage = ({ title, data, footer }) => {
    React.useEffect(() => {
        document.title = title
    }, [title])
    return (
        <article>
            <h1>{title}</h1>
            <p>{data}</p>
            <span>{footer}</span>
        </article>
    )
}
{{< /highlight >}}

Este es un sencillo componente que renderiza el contenido de un "post" y define el título de la página como el título del post. Para esto utiliza \`useEffect\` que ejecuta el callback definido: cambiar el título del documento, cada vez que la prop \`title\` cambia.

Ahora, esta es una tarea común - cambiar el título documento - que también se realiza en la sección videos, about, etc. Entonces tiene sentido querer extraer esta lógica. Para eso creamos un nuevo hook:

{{< highlight javascript "linenos=table, linenostart=1" >}}
const useUpdateDocumentTitle = (title) => {
    React.useEffect(() => {
        document.title = title
    }, [title])
}
{{< /highlight >}}

Ahora podemos guardar este nuevo hook en un modulo diferente, llamemoslo \`hooks.js\` y al volver a nuestro componente \`PostPage\` lo refactorizamos para que utilice nuestro grandioso hook

{{< highlight jsx "linenos=table, linenostart=1" >}}
const PostPage = ({ title, data, footer }) => {
    useUpdateDocumentTitle(title)
    return (
        <article>
            <h1>{title}</h1>
            <p>{data}</p>
            <span>{footer}</span>
        </article>
    )
}
{{< /highlight >}}

Como vez, extraer lógica compartida y crear una simple abstracción utilizando una función que contenga esta lógica junto con los hooks correspondientes es relativamente sencillo.
Primero, debes identificar la lógica que se requiere reutilizar, luego extraer y refactorizar tu componente para que haga uso del nuevo hook.

Puedes encontrar muchos hooks que han sido empaquetados para compartirlos en \`npm\`. Un buen listado es el que puedes encontrar en <https://usehooks.com/>
