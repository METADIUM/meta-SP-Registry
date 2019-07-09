# Server side Authentication Process
Explanation of Server side authentication for each language settings with sample codes. 


### Java

[Web3j](https://github.com/web3j/web3j#getting-started) Add. Following codes are based on version 4.2.0.  
For eash of calling Contract method; [IdentityRegistry](https://github.com/YoungBaeJeon/metadium_android_sdk/edit/master/app/src/main/java/com/metadium/metadiumsdk/IdentityRegistry.java), [ServiceKeyResolver](https://github.com/YoungBaeJeon/metadium_android_sdk/edit/master/app/src/main/java/com/metadium/metadiumsdk/IdentityRegistry.java) copy the source and include it in the project. 

```java
// signature util method
public static Sign.SignatureData stringToSignatureData(String signature) {
    byte[] bytes = Numeric.hexStringToByteArray(signature);
    return new Sign.SignatureData(bytes[64], Arrays.copyOfRange(bytes, 0, 32), Arrays.copyOfRange(bytes, 32, 64));
}

final bool IS_TEST_NET = true;
final String OPEN_API_NODE_URL = IS_TEST_NET ? "https://api.metadium.com/dev" : "https://api.metadium.com/prod";    // testnet, mainnet node url
final String IDENTITY_REGISTRY_CONTRACT_ADDRESS = IS_TEST_NET ? "0xbe2bb3d7085ff04bde4b3f177a730a826f05cb70" : "0x42bbff659772231bb63c7c175a1021e080a4cf9d";    // contract address of IdentityRegistry (testnet, mainnet)

String nonce = "...";       // nonce value sent to Keepin App
String sinature = "...";    // Signature value from Keepin App
String metaId = "...";      // Meta ID value fromp Keepin App
String serviceId = "...";   // Registered service id

// Retrieve signer's addres via ec-recover
SignatureData signatureData = stringToSignatureData(signature);
BigInteger publicKey;
try {
    publicKey = Sign.signedMessageToKey(nonce.getBytes(), signatureData);
}
catch (SignatureException e) {
    // invalid signature
    return;
}
String key = Numeric.prependHexPrefix(Keys.getAddress(publicKey));

// convert metaId to ein
BigInteger ein = Numeric.toBigInt(metaId);

// Obtain resolver address from IdentityRegistry
Web3j web3j = Web3j.build(new HttpService(OPEN_API_NODE_URL));
IdentityRegistry identityRegistry = IdentityRegistry.load(
        IDENTITY_REGISTRY_CONTRACT_ADDRESS,
        web3j,
        new ReadonlyTransactionManager(web3j, null),
        new StaticGasProvider(BigInteger.ZERO, BigInteger.ZERO)
);
String resolverAddress;
try {
    Tuple4<String, List<String>, List<String>, List<String>> identity = identityRegistry.getIdentity(ein).send();
    if (identity.getValue4().size() > 0) {
        resolverAddress = identity.getValue4().get(0);
    }
    else {
        // not exists resolver
    }
}
catch (Exception e) {
    // not found identity or call error
    return;
}

// Check for Key Registration
ServiceKeyResolver serviceKeyResolver = ServiceKeyResolver.load(
        resolverAddress,
        web3j,
        new ReadonlyTransactionManager(web3j, null),
        new StaticGasProvider(BigInteger.ZERO, BigInteger.ZERO)
);
try {
    boolean hasForKey = serviceKeyResolver.isKeyFor(key, ein).send();
    String symbol = serviceKeyResolver.getSymbol(key).send();

    if (hasForKey) {
        if (serviceId.equalsIgnoreCase(symbol)) {
            // Key is registered
        }
        else {
            // Key is registered, but service hasn't been registered
        }
    }
    else {
        // Key is not registered
    }
}
catch (Exception e) {
    // error
}
```
  
  
  
### JavaScript
```javascript
import Web3 from 'web3';

const IS_TESTNET = true;
const OPEN_API_NODE_URL = IS_TEST_NET ? 'https://api.metadium.com/dev' : 'https://api.metadium.com/prod';        // testnet, mainnet node url
const IDENTITY_REGISTRY_CONTRACT_ADDRESS = IS_TEST_NET ? '0xbe2bb3d7085ff04bde4b3f177a730a826f05cb70' : '0x42bbff659772231bb63c7c175a1021e080a4cf9d';    // contract address of IdentityRegistry (testnet, mainnet)

let nonce = "...";       // nonce value sent to Keepin App
let sinature = "...";    // Signature value from Keepin App
let metaId = "...";      // Meta ID value from Keepin App
let serviceId = "...";   // Registered service id

// init web3
const web3 = new Web3();
web3.setProvider(new Web3.providers.HttpProvider(OPEN_API_NODE_URL));

// ec-recovery
const key = web3.accounts.recover(web3.utils.sha3(nonce), signature, true);

// Obtain resolver address from IdentityRegistry
const identityRegistryContract = web3.eth.Contract(ABI_CODE, IDENTITY_REGISTRY_CONTRACT_ADDRESS);
const identity = await identityRegistryContract.methods.getIdentity(ein).call();
const serviceKeyResolverAddress = identity.resolvers[0];

// Check for Key Registration
const serviceKeyResolverContract = web3.eth.Contract(ABI_CODE, serviceKeyResolverAddress);
const hasKey = await serviceKeyResolverContract.methods.isKeyFor(key, ein).call();
const symbol = await serviceKeyResolverContract.methods.getSymbol(key).call();
if (hasKey) {
    if (symbol.equals(serviceId)) {
	    // Key is registered
    }
    else {
        // Key is registered, but serivce hasn't been registered
    }
}
else {
	// Key is not registered
}
```
