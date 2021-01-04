+++
title = "WTF is a Closure in JS"
author = ["Matias Hernandez"]
draft = false
+++

-   tags:: [Javascript]({{< relref "20200927000418-javascript" >}})


## Closure son confusas por que son un concepto "invisible" {#closure-son-confusas-por-que-son-un-concepto-invisible}

A diferencia de otros conceptos como funciones, variables u otros. Los closures no son utilizados a conciencia. no dices "Oh aquí usaré un closure como solución"


### Closure son diferentes {#closure-son-diferentes}

lo más probable es que ya lo has usado cientos de veces. Aprender closure es más sobre identificar cuando lo estas utilizando más que un nuevo concepto en sí.


## TLDR {#tldr}

Tienes una closure cuando una función cualquiera acceder a una variable fuera de su scope.

```javascript
const value = 1
function doSomething() {
    let data = [1,2,3,4,5,6,7,8,9,10,11]
    return data.filter(item => item % value === 0)
}

```

Aqui la función \`doSomething\` hace uso de \`value\`. Pero también la función \`item => item % value ===0\` que también puede ser escrita como

```javascript
function(item){
    return item % value === 0
}

```

utiliza el valor de la variable \`value\` que fue definida fuera de la función.


## Las funciones pueden acceder valores fuera de su contexto {#las-funciones-pueden-acceder-valores-fuera-de-su-contexto}

Como en el ejemplo anterior, una función puede acceder y utilizar valores que están definidos fuera de su "cuerpo" o contexto, por ejemplo

```javascript
let count = 1
function contador() {
    console.log(count)
}
contador() // imprime 1
count = 2
contador() // imprime 2

```

Esto, permite que podamos modificar el valor de la variable \`count\` desde cualquier parte del módulo y, cuando la función contador sea llamada, sabrá usar el valor actual.


## Por que usamos funciones {#por-que-usamos-funciones}

Pero, por que utilizamos funciones en nuestros programas?, Ciertamente es posible - dificil, pero posible - escribir un programa sin utilizar funciones definidas por nosotros mismos, entonces por que creamos funciones propias?

Imagina un trozo de código que hace **algo maravilloso** , lo que sea, y está compuesto por X número de lineas

```javascript
/* Mi trozo de codigo maravilloso */
```

Ahora supon que debes utilizar este **trozo de codigo maravilloso** en varias partes de tu programa, ¿Qué harías?.
La opción "natural" es juntar este trozo de código en un conjunto que pueda ser reutilizable, y ese conjunto reutilizable es lo que llamamos función. Las funciones son la mejor forma de reutilizar y compartir código dentro de un programa.

Ahora, puedes utilizar tu función cuantas veces sean y, ignorando algunos casos particular, llamar tu función N veces es lo mismo que escribir ese **trozo de código maravilloso** N veces. Es un simple reemplazo.


## ¿Pero donde está el closure aquí? {#pero-donde-está-el-closure-aquí}

Usando el ejemplo del contador, consideremos ese como el **trozo de código maravillos**

```javascript
let count = 1
function contador() {
    console.log(count)
}
contador() // imprime 1

```

Ahora, queremos reutilizarlo en muchas partes

```javascript
function marvel() {
    let count = 1
    function contador() {
        console.log(count)
    }
    contador() // imprime 1
}

```

ahora, que tenemos?
Una función: \`contador\` que utiliza un valor que fue declarado fuera de ella \`count\`.
y un valor: \`count\` que fue declarado en la función \`marvel\` pero que es usado dentro de la función \`contador\`.

Es decir, tenemos una función que utiliza un valor que fue declarado fuera de su contexto: un closure.

Simple no? Ahora, que pasa cuando se ejecuta la función \`marvel\`?, que ocurre con la variable \`count\` y la función \`contador\`?
Una vez ejecutada la función **padre**, las variables y funciones declaradas en su cuerpo "desaparecen" (garbage collector).

Ahora, modifiquemos un poco el ejemplo:

```javascript
function marvel() {
    let count = 1
    function contador() {
        count++
        console.log(count)
    }
   setInterval(contador, 2000)
}
marvel()
```

¿Que ocurrirá ahora con la variable y funcion declaradas dentro de \`marvel\`?
En este ejemplo, le decimos al browser que ejecute \`contador\` cada 2 segundos. Por lo que el engine del browser debe mantener una referencia a la función y tambien a la variable que es utilizada por ella. Entonces, incluso una vez que la función padre \`marvel\` terminara su ciclo de ejecución, la función \`contador\` y el valor \`count\` seguiran "viviendo".

Este "efecto" de tener closures ocurre por que javasceipt soporta la anidación de función, o en otras palabras. Las funciones son ciudadanos de primera clase en el lenguaje y pueden ser utilizadas como cualquier otro objeto: anidadas, pasadas como argumento, como valor de retorno, etc.


## Que puedo hacer con closures {#que-puedo-hacer-con-closures}


### Immediately-invoked Function Expression (IIFE) {#immediately-invoked-function-expression--iife}

Es una ténica utilizada mucho en los días de ES5 para implementar el patrón de diseño de "modulo" (antes de que esto fuese soportado nativamente).
 La idea es "envolver" tu módulo en una función que es inmediatamente ejecutada

```javascript
(function(arg1, arg2){
...
...
})(arg1, arg2)
```

Esto permitía el uso de variables privadas que sólo podian ser utilizadas por el propio modulo dentro de la función, es decir, permitía emular los modificadores de acceso

```javascript
const module = (function(){
function metodoPrivado () {
}
const valorPrivado = "algo"
return {
  get: valorPrivado,
  set: function(v) { valorPrivador = v }
}
})()
var x = module()
x.get() // "algo"
x.set("Otro valor")
x.get() // "otro valor"
x.valorPrivado //Error
```

-   Function Factory
    Otro patron de diseño implementado gracias a los closures es la "Fabrica de Funciones",  funciones que crean funciones u objetos, por ejemplo, una función que te permite crear objetos usuarios

    ```javascript
    const createUser = ({ userName, avatar }) => ({
      id: createRandomIdToUseInDatabase(),
      userName,
      avatar,
      changeUserName (userName) {
        this.userName = userName;
        return this;
      },
      changeAvatar (url) {
        // ejecuta logica para obtener el avatar desde la url
        const neAvatar = getAndStore(url)
        this.avatar = newAvatar
        return this
      }
    });

        console.log(createUser({ userName: 'Bender', avatar: 'bender.png' }));

    {
      "id":"17hakg9a7jas",
      "avatar": "bender.png",
      "userName": "Bender",
      "chageUserName": [Function changeUserName]
      "changeAvatar": [Function changeAvatar]

    }
    */

    ```

    Y utilizando este patrón, es posible implementar una idea proviniente de la programación funcional: Currying


#### Currying {#currying}

Es un patrón de diseño ( y característica de algunos lenguajes ) en donde una función, es inmediatamente evaluada y retorna una segunda función.
Este patrón permite ejecutar especialización y composición.
Estas funciones "curried" son creadas utilizando closures, definiendo y retornando la función interna del closure

```javascript
function multiply(a) {

    return function (b) {
        return function (c)  {
            return a * b * c
        }
    }
}
let mc1 = multiply(1);
let mc2 = mc1(2);
let res = mc2(3);
console.log(res);

let res2 = multiply(1)(2)(3);
console.log(res2);
```

Este tipo de funciones toman un solo valor o argumento y retornan otra función que también recibe un argumento. Es una aplicación parcial de los argumentos. También es posible reesscribir este ejemplo utilizando ES6

```javascript
let multiply = (a) => (b) => (c) => {

    return a * b * c;
}

let mc1 = multiply(1);
let mc2 = mc1(2);
let res = mc2(3);
console.log(res);

let res2 = multiply(1)(2)(3);
console.log(res2);
```

Donde podemos aplicar el uso de currying, en composición, digamos que tienes una función que crea elementos html

```javascript

function createElement(element){
    const el = document.createElement(element)
    return function(children) {
        return el.textNode = children
    }
}

const bold = createElement('b')
const italic = createElement('i')
const contenido = 'Mi contenido'
const myElement  = bold(italic(contenido)) // <b><i>Mi contenido</i></b>
```


#### Event Listeners {#event-listeners}

Otro lugar en donde puedes utilizar y aplicar closures es en los manejadores de eventos utilizando React.

Supongamos que estas utilizando una librería de terceros para renderizar los items de tu colección de datos, esta libraríe expone un componente llamado \`RenderItem\` que tiene sólo una prop disponible \`onClick\`. Esta prop no recibe parámetro alguno y tampoco retorna un valor.

Ahora, en tu particular app, requieres que al hacer click en el item se muestre una alerta con el título del item, pero el evento onClick que tienes dispobile no acepta argumentos, que puedes hacer?
Closures al rescate

```jsx
// Esta es el closure
// en es5
function onItemClick(title) {
    return function() {
      alert("Click en " + title)
    }
}
// en es6
const onItemClick = title => () => alert(`Click en ${title}`)

return (
  <Container>
{items.map(item => {
return (
   <RenderItem onClick={onItemClick(item)}>
    <Title>{item.title}</Title>
  </RenderItem>
)
})}
</Container>
)

```


## Conclusión {#conclusión}

Si bien puedes desarrollar una app sin siquiera saber que estas utilizando closures, el conocer su existencia, definición y uso desbloquea nuevas posibilidades a la hora de crear una solución. Closures es uno de esos conceptos que se complican en entender cuando estás empezando, pero hacer el intento de utilizarlos con conocimiento puede permitirte aumentar tus herramientas y avanzar en tu carrera.
