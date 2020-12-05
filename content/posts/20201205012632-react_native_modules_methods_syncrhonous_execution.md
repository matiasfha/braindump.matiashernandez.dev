+++
title = "React Native Modules methods syncrhonous execution"
author = ["Matias Hernandez"]
draft = false
+++

-   tags:: [React Native]({{< relref "20201001102454-react_native" >}})

Un módulo native puede exponer métodos que retornen valor no sólo por medio del uso de Callback y promesas, si no que también de forma syncrona..

**iOS**

```Objective-C

RCT_EXPORT_BLOCKING_SYNCHRONOUS_METHOD(
  echoString:(NSString *)input
) {
  return input;
}
```

> Same as RCT\_EXPORT\_METHOD but the method is called from JS synchronously on the JS thread, possibly returning a result.
> WARNING: in the vast majority of cases, you should use RCT\_EXPORT\_METHOD which allows your native module methods to be called asynchronously: calling methods synchronously can have strong performance penalties and introduce threading-related bugs to your native modules.
> The return type must be of object type (id) and should be serializable to JSON. This means that the hook can only return nil or JSON values (e.g. NSNumber, NSString, NSArray, NSDictionary).
> Calling these methods when running under the websocket executor is currently not supported.

**Android**

```java
@ReactMethod(isBlockingSynchronousMethod = true)
public String echoString(String input) {
  return input;
}

```

> Whether the method can be called from JS synchronously on the JS thread, possibly returning a result.
> WARNING: in the vast majority of cases, you should leave this to false which allows your native module methods to be called asynchronously: calling methods synchronously can have strong performance penalties and introduce threading-related bugs to your native modules.
> In order to support remote debugging, both the method args and return type must be serializable to JSON: this means that we only support the same args as ReactMethod, and the hook can only be void or return JSON values (e.g. bool, number, String, WritableMap, or WritableArray).
> Calling these methods when running under the websocket executor is currently not supported.
