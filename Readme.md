# What is Keepin ?

Keepin App is an all-in-one identity solution. Verify who you are, provide information about yourself, store and secure your data in one place without losing control of it.




# What Keepin Provides

Keepin can help users to manage ID and Authentication in a single application. Users can experience highly secure  PKI based authentication that outperforms ID/PW + 2 Factor Auth. with simple biometric or PIN authentication. Users are also able to utilize safe storage of mobile device to manage their personal information.



# Steps to Utilize Keepin
#### Download Keepin
* Android
  * [MainNet](https://play.google.com/store/apps/details?id=com.coinplug.metadium) : Install in Google Playstore
  * [TestNet](app/testnet/keepin_1.2.2_testnet.apk) : Download and install in link
* iOS
  * [MainNet](https://itunes.apple.com/app//id1452993752?mt=8) : Install in AppStore
  * TestNet : Preparing

#### Service Registration
To register service, input service information at [ServiceRegistry File](service_registry.md#service-infomaton) and send Pull Request.  
For iOS Services, extra step of registering [scheme register](service_registry.md#ios-pre-register-scheme) is required.

#### SDK Preperation
Download proper [SDK](prepare_sdk.md) for your platform and add code.  
For now only Unity.


#### Server Code Addition
Add codes to existing server code for authentication.  
Reference: [Server SampleCode](server_side_usage.md)
