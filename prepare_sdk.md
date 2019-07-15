# Getting Start

### Unity
Download and import [KeepinSDK](http://u3d.as/1zHk) from Asset store.

Write the code referring to KeepinSDK/Example inside the Asset.  (Refer to KeepinSDK/Example in Asset for writing code.)

See [SDK reference](/doc/unity/class_metadium_KeepinSDK.md)

##### Supported Platform
  * Android 4.3+
  * iOS 11.2+


##### Setting Service ID
Initialize the SDK with the [registered service ID](service_registry.md).  
```C#
KeepinSDK keepinSDK = KeepinSDK.Initialize("service id");
```
##### Request register service
Following are example showing request for service register to Keepin App.
```C#
// Check install
if (keepinSDK.IsInstalled())
{
    // Request service register
    // "message" is text to sign, usually created on the server.
    keepinSDK.Register(new CallbackDelegete(RegisterOnResult), "message");
}
else
{
    // Launch store
    keepinSDK.Install();
}
```
Define Callback function for response.
```C#
static void RegisterOnResult(Result result)
{
    if (result.IsSuccess())
    {
        // success register
        // result.metaID : user MetaID
        // result.sign   : signature message by user's private key
        // result.txID   : transaction hash of register
    }
    else
    {
        // occur error
    }

}
```
##### Request Authentication service
Following are example showing request for service authentication to Keepin App.
```C#
public void OnClickAuthentication()
{
    // Check install
    if (keepinSDK.IsInstalled())
    {
        // Request authentication
        // "message" is text to sign, usually created on the server.
        keepinSDK.Authentication(new CallbackDelegete(AuthenticationOnResult), "message", true);
    }
    else
    {
        // Launch store
        keepinSDK.Install();
    }
}
```
Define Callback function for response.
```C#
static void AuthenticationOnResult(Result result)
{
    if (result.IsSuccess())
    {
        // success authentication
        // result.metaID : user MetaID
        // result.sign   : signature message by user's private key
        // result.txID   : if user register service, return transaction hash of register
    }
    else
    {
        // occur error
    }

}
```


### Android
Work In Progress

### iOS
Work In Progress

