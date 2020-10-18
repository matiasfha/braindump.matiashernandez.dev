+++
title = "Post: Agregando modulos nativos con React Native"
author = ["Matias Hernandez"]
draft = false
+++

-   tags:: [RN - Native Modules]({{< relref "20201016151504-rn_native_modules" >}}) [React Native]({{< relref "20201001102454-react_native" >}})

React Native te permite crear aplicaciones para todo tipo dispositivos móviles utilizando Javascript, esto permite una gran flexiblidad y disminuye la curva de aprendizaje.

React Native permite acceso a diferentes API nativas para los distintos sistemas operativos (Android, iOS), pero en ciertas ocasiones esto no es suficiente y es necesario desarrollar soluciones en código nativo: Java/Kotlin o Object-C/Swift.


## Módulos nativos {#módulos-nativos}

React Native permite que el uso de código nativo para utilizar el potencial de cada plataforma, es una característica avanzada y que requiere algunos conocimientos más allá de Javascript y React, pero si la plataforma no te ofrece alguna característica que requieres, es posible crearla.


### Android {#android}

En el caso de Android, el código nativo puede venir distribuido como una paquete jar o aar o creado manualmente como un módulo dentro de tu aplicación.

Quizás necesitas utilizar un SDK o librería externa, en el caso de paquetes _jar_ o _aar_ puedes agregarlos utilizando [Android Studio](<https://developer.android.com/studio>).

1.  Abre tu proyecto en Android Studio, abre sólo el directorio **android**.
2.  Haz click en \`File > New Module\`

3.  Una ventana flotante se mostará en donde puedes elegir el tipo de módulo que quieres importar, en este caso _.JAR\\/_.AAR/. Luego presiona siguiente

<a id="orgc99e31b"></a>

{{< figure src="./images/import-aar.png" caption="Figure 1: Importar librería en Android Studio" >}}

1.  Ahora abre el archivo \`build.gradle\` de tu app y agrega una nueva linea al bloque de dependencias:

    ```gradle
    dependencies { compile project(":my-library-module") }
    ```

    1.  Haz click en _Sync Project With Gradle Files_.

Es posible que tu nuevo módulo ya implemente lo necesarioa para hacer su API disponible en tu proyecto React Native, en caso de no ser así tendras que hacerlo manualmente

Lo primero es crear un nuevo módulo deentro del proyecto, lo llamaremos \`SDKModule\`

Este nuevo módulo implenta una clase que implementa \`ReactContextBaseJavaModule\`

```java
package com.myapp.sdk;

import com.facebook.react.bridge.Callback;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;

public class SDKModule extends ReactContextBaseJavaModule {
   //constructor
   public SDKModule(ReactApplicationContext reactContext) {
       super(reactContext);
   }
   @Override
   public String getName() {
       return "SDK";
   }
   //Custom function that we are going to export to JS
   @ReactMethod
   public void getDeviceName(Callback cb) {
       try{
           cb.invoke(null, android.os.Build.MODEL);
       }catch (Exception e){
           cb.invoke(e.toString(), null);
       }
   }
}
```

Esta clase debe implementar el método \`getName\`. Luego tendrás que agregar los métodos que quieres exponer para su uso en Javascript. Estos métodos deben ser deecorados con la etiqueta \`@ReactMethod\`.

En este ejemplo el método \`getDeviceName\` podrá ser utilizando desde tu código Javascript.

Pero falta un paso más. Es necesario crear un \`package\` con el nuevo módulo. Esta nueva clase permitirá el registro del módulo. Para esto bastará con crear un nuevo archivo llamado \`SDKPackage\`

```java
package com.myapp.sdk;

import com.facebook.react.ReactPackage;
import com.facebook.react.bridge.JavaScriptModule;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.ViewManager;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class SDKPackge implements ReactPackage {

   @Override
   public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
       return Collections.emptyList();
   }

   @Override
   public List<NativeModule> createNativeModules(
           ReactApplicationContext reactContext) {
       List<NativeModule> modules = new ArrayList<>();
       //We import the module file here
       modules.add(new SDKModule(reactContext));

       return modules;
   }

   // Backward compatibility
   public List<Class<? extends JavaScriptModule>> createJSModules() {
       return new ArrayList<>();
   }
}
```

Finalmente debemos registrar el paquete en la clasee principal \`MainApplication.java\`

```java
    import com.notetaker.sdk.SDKPackage;

   @Override
   protected List<ReactPackage> getPackages() {
     return Arrays.<ReactPackage>asList(
         new MainReactPackage(),
         new SDKPackage() //Add your package here
     );
   }
 };
```

Listo, ahora tu nuevo módulo estará disponible dentro del objeto \`NativeModules\` en tu app React Native, bajo el nombre que definiste en el método \`getName\`.

```javascript
import {NativeModules} from 'react-native';
NativeModules.SDK.getDeviceName((err ,name) => {
   console.log(err, name);
});
```


## Conclusión {#conclusión}

React Native es una plataforma que permite el desarrollo rápido y seguro de aplicaciones móviles, pero no tiene soporte (aún) para cada una de las caracterísitcas de los dispositivos o a veces el soporte ofrecido por defecto no es suficiente, en estos casos querrás crear un módulo nativo, que no más que código Java - en el caso de Android -  que te permite definir como utilizar cierta caracterísitca.
Este código puede ser expuesto a tu aplicación Javascript tal como se describe en el ejemplo.
