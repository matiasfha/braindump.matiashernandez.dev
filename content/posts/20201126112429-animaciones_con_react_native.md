+++
title = "Animaciones con React Native"
author = ["Matias Hernandez"]
draft = false
+++

-   tags:: [React Native]({{< relref "20201001102454-react_native" >}})

Las animaciones son parte importante de cualquier aplicación ya que permiten ofrece una mejor experiencia de uso siendo utilizadas como feedback para las acciones del usuario.
React Native ofrece una API para trabajar con animaciones directamente utilizando "just javascript".

En este post revisaremos como utilizar esta caracterísitca para crear tus propias animaciones!.


## La API Animated {#la-api-animated}

React Native ofrece dos API complementarias: \`Animated\` para un control granular de valores específicos y \`LayoutAnimation\` para efectos animados globales en transacciones del layout.

La API \`Animated\` fue diseñada para ofrecer control granular para crear diferentes patrones de interaccion y con la mejor performance posible, es una api declarativa que permite control de inicio y fin de una animación.

Esta API exporta seis componentes para animar, pero también permite crear tu propio componente utilizando \`Animateed.createAnimatedComponent()\`.
Los componentes animados por defecto son: \`View\`, \`Text\`, \`Image\`, \`ScrollView\`, \`FlatList\` y \`SectionList\`

Asi mismo, \`Animated\` ofrece métodos para controlar los valores que estás animando como \`Animated.timing\` y \`Animated.spring\` y otros que permiten componer animaciones como: \`Animated.parallel\`, \`Animated.sequence\`, \`Animated.delay\` y \`Animated.loop\`.

Primero vamos a revisar algunos de estos métodos:


### Animated.timing y Animated.spring {#animated-dot-timing-y-animated-dot-spring}

Estos métodos permiten definir una animación, \`timing\` permite definir un tiempo para ejecutar la animación y \`spring\` utiliza un modelo físico para determinar la velocidad para ejecutar la animación, es decir, no es controlado por el tiempo.
Ambos métodos tienen una api similar aceptando casi los mismos parámetros

Revisemos un ejemplo

<div data-snack-id="@matiasfh/example-translate-animation" data-snack-platform="web" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#F9F9F9;border:1px solid var(--color-border);border-radius:4px;height:505px;width:100%"></div>
<script async src="<https://snack.expo.io/embed.js>"></script>

En este ejemplo podemos ver dos elementos y dos animaciones, una con \`timing\` y la otra con \`spring\`.

Revisemos el código.
Lo primero que podemos ver dentro de la definicion de \`App\` es el uso de \`React.useRef()\`

```javascript
const translateAnimationTiming = React.useRef(new Animated.Value(0)).current
```

Aqui utilizamos \`useRef\` para mantener una referencia a un valor sin necesidad de emitir un cambio de estado provocando un re-renderizado del componente, dentro del \`ref\` creamos un valor para animar utilizando \`new Animated.Value(0)\` iniciando que el valor inicial será 0, luego definimos la función que iniciará la animación, en este caso es un manejador para el evento click de los botones

```javascript
const timingAnimation = () => {
    Animated.timing(translateAnimationTiming, {
      toValue: 250,
      duration: 600,
      useNativeDriver: true,
    }).start()
}
```

Esta función declara y ejecuta la animación \`timing\`. El primer argumento es el valor que se animará y luego recibe un objeto de configuración \`toValue\` que declara que se animará hasta obtener el valor 250 en un tiempo \`duration: 600\` (milisegundos). y definimos que se usará el driver nativo. Este último argumento es requerido por React Native como \`true\` or \`false\`.

Sólo algunos valores pueden ser animados utilizando el driver nativo, tal como se describe en [este post de la documentación oficial](<https://reactnative.dev/blog/2017/02/14/using-native-driver-for-animated>), los valores animables por el driver nativo son todos aquellos "no relacionados con el layout", es decir no puedes animar nativamente (pero si al utilizar \`useNativeDriver: false\`) propiedades de flexbox, tamaños (width, height) o posiciones.

En este ejemplo también hay otra función que ejecuta la animación utilizando \`spring\`

```javascript
const timingAnimation = () => {
    Animated.spring(translateAnimationTiming, {
      toValue: 250,
      useNativeDriver: true,
    }).start()
}
```

El método spring solo recibe el valor al cual se quiere animar (sin tiempo).
Puedes ver la diferencia entre ambas animaciones en el ejemplo de Snack.

Por cierto, en ambos casos se hizo una llamada al método \`start()\` para iniciar la animación. \`start()\` puede recibir un argumento. Un callback que es ejecutado cuando la animación terminó. Si la animación termino de forma correcta el callback será invocado con el argumento \`{finished:true}\`, en caso contrario, si la animación, termino de forma erronea, o no se pudo completar (por ejemplo, por alguna interrupción al re-renderizar), el callback recibirá el parámetro \`{finished:false}\`


### Componentes animados {#componentes-animados}

Otro punto importante del ejemplo es el uso de componentes animados, en este caso \`Animated.View\` este es un componente ofrecido por la API Animated que tiene el mismo comportamiento y props que el \`View\` normal, pero contiene la lógica necesaria para efectuar los cambios en los estilos determinados por la función de animación.

```javascript
<Animated.View
    style={[
    styles.circle,
    animatedStyleTiming,
    ]}
/>

```

La animación es en el fondo, una modificación a alguna propiedad del estilo del componente, en este caso \`animatedStyleTiming\` que para este ejemplo está definido como

```javascipt
const animatedStyleTiming = {
    transform: [{ translateX: translateAnimationTiming}]
}
```

Este style, define la propiedad transform para indicar que se modificará la posición en \`X\` del elemento.


### Easing {#easing}

Easing functions o funciones de aceleración especifican la forma o monto del cambio de un parámetro sobre durante el tiempo.
Los objetos, en la vida real no inician o detienen su movimiento de forma abrupta, y casi nunca se mueven a una velocidad constante.
React Native ofrece varias funciones de aceleración para aplicar en nuestras animaciones, para su uso simplemente debes importar el objeto \`Easing\`, como en el siguiente ejemplo

<div data-snack-id="@matiasfh/rn-easing-examples" data-snack-platform="web" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#F9F9F9;border:1px solid var(--color-border);border-radius:4px;height:505px;width:100%"></div>
<script async src="<https://snack.expo.io/embed.js>"></script>

En este pequeño proyecto puedes ver la aplicación de las diferentes funciones de aceleración disponibles

```javascript
import { Easing } from 'react-native';
...
...
Animated.timing(animatedValue, {
  ...
  easing: Easing.ease
}).start()
```

Para aplicar una función de aceleración a tu animación solo debes definir la propiedad \`easing\` en la declaración de la animación.


### Interpolation {#interpolation}

Otro método que React Native dispone para controlar nuestras animaciones es la interpolación.
Cada propeidad definida para animar puede pasar por un proceso de interpolación primero, este proceso crea un mapa de entradas y salidas \`inputRange\` y \`ouputRange\`.
Esto te permite utilizar tu valor de animación varias veces incluso si solo está definido como un valor entre \`0\` y \`1\` al esquematizar tu rango de valores con un rango de la propiedad que quieres animar.
Vamos un ejemplo y animemos dos propiedades diferentes de dos componentes.

<div data-snack-id="@matiasfh/rn-animation-interpolation-example" data-snack-platform="web" data-snack-preview="true" data-snack-theme="light" style="overflow:hidden;background:#F9F9F9;border:1px solid var(--color-border);border-radius:4px;height:505px;width:100%"></div>
<script async src="<https://snack.expo.io/embed.js>"></script>

En este ejemplo tenemos dos componentes animados. Un circulo rojo que se mueve hacia la derecha y un circulo azul que "desaparece" cambiando su opacidad, pero solo tenemos la definición de un valor de animación

```javascript
const animatedValue = react.useRef(new Animated.Value(0)).current
```

Valor de animación que comienza en 0 y que manipularemos para que cambie siemppre entre valores 0 y 1.
También se definen dos interpolaciones \`translateInterpolation\` que genera un mapa de 0 a 0 y 1 a 250. Y \`opacityInterpolation\` que crea el mapa reverso 0 -> 1, y 1 -> 0. Indicando que cuando el valor de animación sea 0, el valor utilizado por la interpolación será \`1\`.

```javascript
  const translateInterpolation = animatedValue.interpolate({
    inputRange: [0,1],
    outputRange:[0,250]
  })
  const opacityInterpolation = animatedValue.interpolate({
    inputRange: [0,1],
    outputRange:[1,0]
  })
```

También se modifica la función de animación para indicar que el valor de cambio será \`toValue: 1\`

```javascript
const animation = () => {
    animatedValue.setValue(0)
    Animated.timing(animatedValue, {
        toValue: 1,
        duration: 600,
        useNativeDriver: true,
    }).start()
 }
```

Entonces al hacer click en el botón **Animate** ambos componentes son animados, uno se mueve \`250px\` a la derecha mientras el otro cambia su opacidad de 1 a 0.


## Conclusión {#conclusión}

React Native provee de un control granular a la hora de crear animaciones permitiendo que sea solo tu imaginación el limite para definir como quieres que tu interfaz se comporte.
Para poder animar componentes necesitas utilizar los componentes animdos provistos en el objeto \`Animated\` y luego definir un valor de animación utilizando \`useRef\`. Este valor de animación es despues utilizado como parámetro para la función de animación que quieres utilizar como \`timing\` o  \`spring\`.
Además ofrece una forma de control aún mas detallada como la interpolación, que permite crear un mapa de valores pudiendo reutilizar el valor de animación como se desee.
