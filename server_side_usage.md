# Server side Authentication Process
Explanation of Server side authentication for each language settings with sample codes. 


### Java
Need [Metadium did-resolver-java-client](https://github.com/METADIUM/did-resolver-java-client). 


```java
String nonce = "...";       // nonce value sent to Keepin App
String sinature = "...";    // Signature value from Keepin App
String metaId = "...";      // Meta ID value fromp Keepin App

final bool IS_TEST_NET = true;

// Make did with Meta ID
final String did = (IS_TEST_NET ? "did:meta:" : "did:meta:testnet:")+metaId;

// Get did document
DidDocument didDocument = DIDResolverAPI.getInstance().getDocument(did, true);
if (didDocument == null) {
	// TODO Not found did document
	return;
}

// Check if did document has address with signature
boolean verified = didDocument.hasRecoverAddressFromSignature(nonce.getBytes(StandardCharsets.UTF_8), signature);

```

  
### JavaScript
Need [Web3js](https://web3js.readthedocs.io/en/v1.2.2/getting-started.html).

Will add did-resolver later.

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
