# Metadium Namespace Reference
***class*** [**KeepinSDK**](class_metadium_KeepinSDK.md#KeepinSDK-Class-Reference)

***class*** [**Result**](class_metadium_KeepinSDK.md#result-class-reference)

***enum*** [**ErrorCode**](class_metadium_KeepinSDK.md#ErrorCode)

***delegate*** ***void*** [**CallbackDelegete**](class_metadium_KeepinSDK.md#callbackdelegete)([**Result**](class_metadium_KeepinSDK.md#result-class-reference) result)


ErrorCode
-------------
***enum*** **ErrorCode** : ***int***

Response error code

| Error code name     | description                                                                                |
|---------------------|--------------------------------------------------------------------------------------------|
| UserCancel          | User cancaled                                                                              |
| NotCreatedMetaID    | MetaID not created                                                                         |
| NotMatchedMetaID    | Requesting MetaID is not match user MetaID.<br>This can happen if vale of expectMetaId in [Authentication()](class_metadium_KeepinSDK.md#authentication) is not null.                                                          |
| NotlinkedService    | Service is not enabled by use                                                              |
| NotRegisterService  | Service is not register by Metadium.<br> See [service register](../../service_registry.md) |
| InvalidParam        | Invalid parameter value                                                                    |
| InvalidSignature    | *Not used*                                                                                 |

CallbackDelegete()
----------------------
***delegate void*** **Metadium.CallbackDelegete**(**[Result](class_metadium_KeepinSDK.md#callbackdelegete)** result)

callback delegate for receiving response to requests to register and authentication to Keepin App.

*Parameters*
  * result : Response to request


## KeepinSDK Class Reference

Authentication()
--------------------------------------------------------
***void*** **Authentication**(**[CallbackDelegete](class_metadium_KeepinSDK.md#callbackdelegete)** callback, ***string*** nonce, ***bool*** autoRegister = `false`, ***string*** expectMetaId = `null` )

Request authentication of the service in Keepin App.

*Parameters*
  * callback : Callback delegate to receive response to authentication. **required**
  * nonce : Data to sign with the service key. **required**
  * autoRegister : If service is not registered and this value is true, request to register. **optional default false**
  * expectMetaId : Expect Keepin's Meta ID to match the given Meta ID. If IDs are not same, error code reponse with NotMatchedMetaID(2). **optional default null**

*Throws*
  * callback or nonce is null

Initialize()
----------------------------------------------------
***static*** **[KeepinSDK](class_metadium_KeepinSDK.md#KeepinSDK-Class-Reference)** **Initialize**(***string*** serviceId)

SDK initializing

*Parameters*
  * serviceId : Service ID assigned by Metadium

*Returns*
  * SDK instance

*Throws*
  * ServiceId is null or empty string

Install()
-------------------------------------------------
***void*** **Install**()

Launch store to download Keepin app

IsInstalled()
-----------------------------------------------------
***bool*** **IsInstalled()**

Check to installed Keepin App.

*Returns*
  * if installed keepin app, return true

OnError()
-------------------------------------------------
***void*** **OnError**(***string*** message)
  
On response error
  
*Parameters*
  * message : error message

OnResult()
--------------------------------------------------
***void*** **OnResult**(***string*** message)
  
On response data  
  
*Parameters*
  * message : uri for iOS, json for android. If platform is iOS, this function call error response.  

Register()
--------------------------------------------------
***void*** **Register**(**[CallbackDelegete](class_metadium_KeepinSDK.md#callbackdelegete)** callback, ***string*** nonce)
  
Request registration of the service in Keepin App.  
  
*Parameters*
  * callback : Callback delegate to receive response to register  
  * nonce : Data to sign with the service key

## Result Class Reference

Result()
--------------------------
**Result**(***int*** code)

Contruct with Error code

*Parameters*
  * code : error code

IsSuccess()
---------------
***bool*** **IsSuccess**()

Check success result

*Returns*
  * If error code is success, return true

metaID
----------------
***string*** metaID
  
Meta ID of Keepin

sign
----------------
***string*** sign

Meta ID of Signature with service key  

txID
----------------
***string*** txID

Service registered transcion hash

code
----------------
***int*** code = -1

Error Code. Show [ErrorCode](class_metadium_KeepinSDK.md#ErrorCode)

error
----------------
***string*** error

Error message
