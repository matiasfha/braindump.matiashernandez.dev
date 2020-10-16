+++
title = "RN - Native Modules"
author = ["Matias Hernandez"]
draft = false
+++

-   tags::[React Native]({{< relref "20201001102454-react_native" >}})

-   Un modulo nativo es codigo nativo de cada plataforma que puede ser accedido desde el código Javascript.


## Android {#android}

este código nativo puede venir como un paquete jar, aar o creado manualmente.

En el caso de paquetes jar o aar, puedes agregarlo utilizando Android Studio.

-   Abre el proyecto en Android Studio. Abrir el direction \`android\`
-   Haz clic en File > New Module.
-   Haz clic en Import .JAR/.AAR Package y luego en Next
-   Asegura que la biblioteca se ubique en la parte superior de tu archivo settings.gradle, como se muestra aquí para una biblioteca llamada “my-library-module”:

-   include ':app', ':my-library-module'
-   Abre el archivo build.gradle del módulo de la app y agrega una línea nueva al bloque de dependencies como se muestra en el siguiente fragmento:
    dependencies {  compile project(":my-library-module") }

-   Haz clic en Sync Project with Gradle Files para sincronizar el proyecto con los archivos gradle.

Si el nuevo modulo no implementa el código necesario para el bridge, debes agregarlo manualmente.

Ej: Tienes una libreria llamada Client

-   Ejecuta los pasos anteriores para agregarla al proyecto
-   Crea una nueva clase Java dentro del proyecto, llamada  \`ClientModule\`

    esta clase debe implementar \`ReactContextBaseJavaModule\`
    ejemplo

    ```java
    package com.myapp.client;

    import com.facebook.react.bridge.Callback;
    import com.facebook.react.bridge.ReactApplicationContext;
    import com.facebook.react.bridge.ReactContextBaseJavaModule;
    import com.facebook.react.bridge.ReactMethod;

    public class ClientModule extends ReactContextBaseJavaModule {
       //constructor
       public DeviceModule(ReactApplicationContext reactContext) {
           super(reactContext);
       }
       //Mandatory function getName that specifies the module name
       @Override
       public String getName() {
           return "ClientSDK";
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

    La nueva clase debe implementar (\`@Override\`) el método \`getName\`, luego cualquier método que quieras esté disponible para tu uso en Javacript debe ser decorado con \`@ReactMethod\`.

En este caso el método \`getDeviceName\` estará disponible en tu código Javascript como

```javascript
import {NativeModules} from 'react-native';
NativeModules.ClientSDK.getDeviceName((err ,name) => {
   console.log(err, name);
});
```

-   Es en este archivo en donde escribirás todos los métodos necesariospara hacer el bridge con tu librería externa \`Client\`
-   Ahora es necesario registrar el nuevo modulo. Crea una nueva clase llamada \`ClientPackage\`

    ```java
    package com.notetaker.device;

    import com.facebook.react.ReactPackage;
    import com.facebook.react.bridge.JavaScriptModule;
    import com.facebook.react.bridge.NativeModule;
    import com.facebook.react.bridge.ReactApplicationContext;
    import com.facebook.react.uimanager.ViewManager;
    import java.util.ArrayList;
    import java.util.Collections;
    import java.util.List;

    public class ClientPackage implements ReactPackage {

       @Override
       public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
           return Collections.emptyList();
       }

       @Override
       public List<NativeModule> createNativeModules(
               ReactApplicationContext reactContext) {
           List<NativeModule> modules = new ArrayList<>();
           //We import the module file here
           modules.add(new ClientModule(reactContext));

           return modules;
       }

       // Backward compatibility
       public List<Class<? extends JavaScriptModule>> createJSModules() {
           return new ArrayList<>();
       }
    }
    ```

    -   El último paso es registrar el nuevo paquete en tu clase principal. \`MainApplication.java\`

        ```java
        import com.notetaker.device.ClientPackage;

           @Override
           protected List<ReactPackage> getPackages() {
             return Arrays.<ReactPackage>asList(
                 new MainReactPackage(),
                 new ClientPackage() //Add your package here
             );
           }
         };
        ```

Listo, ahora tu nuevo módulo estará disponible como \`NativeModules\` en tu app React Native
