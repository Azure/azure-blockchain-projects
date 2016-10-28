#Project Bletchley - The Cryptlet Fabric
##Cryptlets in Depth 

The promise that blockchain has to revolutionize how business and individuals trade value over the internet is staggering.  Enterprise organizations see the vision for disrupting their industry and themselves.  The atom has been split, so now the race to take advantage of it begins.  

However, there are some challenges for building applications and new ecosystems on distributed ledger technologies.  These are largely identified in the previous <a href=https://github.com/Azure/azure-blockchain-projects/blob/master/bletchley/bletchley-whitepaper.md>Introduction to Bletchley white-paper</a> as well as an approach for building a distributed platform in the cloud to enable these new types of applications.

For example, blockchains will need a way to securely receive external data variables as well as access to secure execution of off-chain code.  Security advances in silicon chips will soon introduce a new level of security into CPU architectures called enclaves.  The use of enclaves allows for execution of code to be performed in a secure isolated container in which the results of the computation are attested to be tamper proof.

Examples:
* Secure data is needed when a SmartContract requires the LIBOR interest rate to perform some calculation, how does it trust that the rate it is using is completely authentic and has not been tampered with?  
	* Receive Market Data based on an event
	* Specific Time i.e. 4:00 PM EST
	* Specific Interval i.e. every 15 minutes
	* Price of something hits a threshold i.e. Oil goes above $40 a barrel
	* Receive data based on external application updates, i.e. CRM System customer credit rating drops
	* Request and Receive results from a High Performance Computing job i.e. Monte Carlo simulation, Gene Sequence completion and results, etc.


* Secure execution of external code would be needed if a SmartContract required that portions of its logic needed to run in completely secure isolation and attested that it was not tampered with during execution. 
	* Secure IP protected algorithms but still share with the blockchain network: i.e. derivative pricing algorithm that multiple counter parties agree to use for a contract, but the actual algorithm remains secret, but attested.
	* Scale an algorithm for maximum performance by running it off the blockchain in a secure and attested way.
	* Privacy of execution, share secrets (terms, keys, data) for transaction completion.
	* Perform complex interactions like distributed transaction coordination across many systems in a secure way.
	* Use libraries for common platforms like Java and .NET in your SmartContracts.

Bletchley promises to deliver an Blockchain Application Fabric that allows enterprises to re-invent business processes in a collaborative way.  Cryptlets exist in this Cryptlet Fabric providing much needed functionality like integration, secure execution, privacy, interoperability, management and a rich set of data services.  Cryptlets are the components that developers can build to address specific sets of functionality that are foundational for building distributed ledger based applications. 

Bletchley's Cryptlet Fabric introduces a secure, attested third tier into blockchain applications. Given the distributed nature of blockchain this Cryptlet Fabric will function as a service or application fabric in the cloud (Azure/Azure Stack, AWS, Google, Private).  Development of the Cryptlet Fabric will provide opportunities to address some of the most concerning limitations of today’s blockchain platforms like performance, scale and security. 

Cryptlets can be consumed as a service in libraries residing in the cloud or created and hosted in a variety of environments.  Cryptlets will be discoverable and published by both developers and SmartContract modelers to include in application design and development experiences.  Microsoft and other 3rd parties will offer Cryptlet libraries that expose functionality that is both horizontal and vertical in nature.  

For example, as a consumer of Cryptlets, you may browse the Cryptlet libraries in the tool of your choice looking for a Cryptlet to provide notification services to awaken your SmartContract based on some event.  You may also want additional functionality from this service like providing market prices upon awakening.  Once you find the Cryptlet you are interested in using, you can simply make a reference to that Cryptlet and use it to subscribe to a notification service providing it with when you want to be awakened and what data you want when you are notified.  The Cryptlet infrastructure will attest to the security, authenticity and reliability of the service and data provided along with digital signatures for the Cryptlet and the attested host for audit.

Additionally, you may want to develop your own Cryptlets that integrate data or logic from an external system like validating customer's data without disclosing it to the blockchain itself.  You can create your own Cryptlet, write it in the language of your choice and deploy to either Azure, Azure Stack or localhost* using the Bletchley SDK. 

![<Event UC>](<images/azureRegions.png>)
*As of September 2016 – Updates https://azure.microsoft.com/en-us/regions/*

As both public and consortium blockchains begin to span the globe, Azure’s global scale can deliver a secure execution and data environment in standard open way.  

Although similar to blockchain oracles, Cryptlets provide much more in the way of security and trust in a scalable ecosystem.

![<Event UC>](<images/cryptletVorcles.png>)     

Providing enterprise grade software with cutting edge advances in hardware, the Cryptlet Fabric makes a great companion layer for cross cutting capabilities so developers and consortiums can focus on solutions driving distributed computing into new frontiers.

This paper explores some of the features and behaviors for Cryptlets in the Project Bletchley Roadmap.  These features and behaviors are under development and subject to radical change.

**Note - Code examples are prototypes and are for demonstration purposes only**

+ [**Utility Cryptlets**](CryptletsDeepDive.md#utility-cryptlets)
+ [**Contract Cryptlets**](CryptletsDeepDive.md#contract-cryptlets)
+ [**Expected Use Cases**](CryptletsDeepDive.md#expected-use-cases)
+ [**Common Characteristics**](CryptletsDeepDive.md#common-characteristics)
+ [**Cryptlet Development**](CryptletsDeepDive.md#cryptlet-development)
+ [**Cryptlet Registration**](CryptletsDeepDive.md#cryptlet-registration)
+ [**CryptletContainers**](CryptletsDeepDive.md#cryptlet-containers)
+ [**Enclaving**](CryptletsDeepDive.md#enclaving)
+ [**Cryptlet Registration**](CryptletsDeepDive.md#cryptlet-registration)
+ [**Conclusion**](CryptletsDeepDive.md#conclusion)


##Overview
Cryptlets are off-chain code modules that are written in any language that can execute within a secure, isolated, trusted container and is communicated with over secure channels.  Cryptlets can be used in SmartContracts and UTXO systems when additional functionality or information is needed, upgrading the blockchain “oracle” approach with a Cryptlet and CryptoDelegate or adaptor.

A CryptoDelegate is the function “hook” within the SmartContract Virtual Machine that calls a Cryptlet from SmartContract code extending the secure and authentic envelop for transactions off the blockchain.  A Cryptlet called via a CryptoDelegate is trusted either by being signed with the digital signature that was trusted and validated when written or at design time.  Each call through the CryptoDelegate checks signatures for validity at runtime and records them along with the transaction.

For example, if a SmartContract subscribes to a notification Cryptlet, that Pub/Sub relationship between the SmartContract and the Cryptlet is established and bound by the digital signatures on both ends.  The CryptoDelegate can enforce the secure communications between the SmartContract and its Cryptlet(s).

For non SmartContract blockchains (UTXO), Cryptlets can be called with adaptors that perform the functions that the CryptoDelegate would in a SmartContract virtual machine, using Opcodes, etc.

Cryptlets can be accessed through a trusted attested host and can expose its source as a property along with a valid trusted hash.  Attested hosting also allows for source not to be revealed, but still trusted in transactions by counter-parties.  The attested host will be communicated with via secure channel (https) with certificate validation throughout the execution environment.  Cryptlets can run within an enclave for secure execution, data, encryption and signing.

SmartContracts using Cryptlets can inherit from one of the base classes in the Bletchley SDK to be able to seamlessly use them.  Then Cryptlets can be called directly or with aspects in the smart contract language and will automatically have the properties wired up to capture Cryptlet signatures and attested hosting metadata.  Aspects, or property tags provide indicators to the virtual machine to delegate execution of certain logic to the CryptoDelegate that then validates Cryptlet communications and/or securely enlists the Cryptlet into the blockchain transaction.

Cryptlets will use enclaves for advanced functionality where process isolation, encryption and advanced threshold signing is required.  Enclaving will be provided by either hardware or software.

Cryptlet developers can write their code on any platform and language. Code compiler verification modules for common platforms like node.js, C#, Java and F# can provide optional determinism, type, security and verification checks to ensure compatibility with the SmartContract platform if required.  Additionally, new languages like F* can also be used to write formally verified Cryptlet code.

Cryptlets can be run in an attested host in Azure, AzureStack or LocalHost.  Localhost support will be for scenarios like dev/test without network access and use cases where Cryptlets will be run on platforms with enclaving support on the validator node (miner, escrow, voter, witness).  The dependencies for running in a localhost enclave will need to be installed on that node for support.

CryptoDelegate “hook” code in the Blockchain Virtual Machine will be open sourced along with the reference specification and implementation of the CryptletContainer and a Cryptlet to provide implementation details to the open source community.

In summary, Cryptlets employ the use of Aspects in Programming Languages, Runtime Integration with CryptoDelegate, Secure computing with Enclaving and the Azure Service Fabric to create a Cryptlet Fabric cloud infrastructure for a host of distributed ledger platforms.

There are two general types of Cryptlets: Utility and Contract and two expected use cases: Event and Control.

## Utility Cryptlets
Utility Cryptlets are organized into libraries or services that perform common functionality like encryption, time based event publication and access to external data.  Utility Cryptlets are used by SmartContract developers who need access to external data or functionality in a trusted way and are referenced at design time.  Utility Cryptlet libraries are provided by the attester and others from 3rd parties like ISVs.  Utility Cryptlets will represent a good portion of “out of the box” Cryptlet Fabric capabilities. (i.e. encryption, authentication, volatile data lookup, notification cryptlets)

Utility Cryptlets usually should be accessed via events that return data required in the return type, this prevents a consensus problem where each node may reach out to a Utility Cryptlet within the same time window and receive volatile data.  

Utility Cryptlets can provide data into Message Queue contract for SmartContracts to reference at their leisure.  For example, you could write a Message Queue SmartContract that subscribes to a Market Data Cryptlet that publishes prices from trusted source every 2 hours when the markets are open.  The Message Queue SmartContract can then be used by other SmartContracts to look up prices when they need them.  The Message Queue SmartContract will maintain the entire history of recorded prices with the signature and hashes for audit and replay.

![<Event UC>](<images/messageQueue.png>)

## Contract Cryptlets
Contract Cryptlets are created and bound to a specific SmartContract instance and is signed by the parent SmartContract and usually runs within an enclave.  The SmartContract using a Contract Cryptlet will inherit property accessors (stored procedures) that implement address checks ensuring that only its Contract Cryptlet can perform certain functions.  The SmartContract and its Contract Cryptlet are bound together with their digital signatures similar to a blockchain wallet.

Contract Cryptlets can host sophisticated logic not entirely suited to run in the EVM for performance, privacy scale purposes. SmartContracts simply contain the state and key variables for the contract (database and accessor methods), while the programming logic, access to external data sources and events of the contract are in its surrogate Cryptlet.  

Contract Cryptlets used with SmartContracts will allow for more advanced scenarios where performance, scalability and privacy are desired.  For example, a Contract Cryptlet can perform the entire operation of a SmartContract in parallel with other Contracts without tying up the EVM.  Using Contract Cryptlets means that each SmartContract does NOT run on all nodes in the blockchain, just those of the counter-parties or attester.

Contract Cryptlets must have at least one instance in the fabric and can have multiple instances in a counter-party Cryptlet pool.  Cryptlets in a pool communicate with each other through the fabric to achieve consensus with each enclave having an equal vote.  One CryptletContainer (enclave) in the pool will record the consensus value to the blockchain. Counter-party pool ledgers can be stored within the SmartContract if desired.

Cryptlets sign their transactions with public/private keys that are issued during registration or creation.  Utility Cryptlets have attested IDs that are made available via a registration process.

Contract Cryptlets are created when a SmartContract is deployed to the blockchain or through the Bletchley SDK.  For example, the constructor of the SmartContract can contain the properties of the Cryptlet as well as the assembly (code) for the targeted platform for the Cryptlet, i.e. C#, Java, C++, etc.

## Expected Use Cases

There are three basic use cases for Cryptlets: Event, Control and Static. Cryptlets are not restricted to only these use cases, these are just the obvious ones.

###Event

Event Driven Cryptlets expose events that SmartContracts can subscribe to.  These Cryptlets provide notification services based on some event criteria and can also optionally pass in secure data upon notification.

![<Event UC>](<images/EventUC.png>)

In this example, the SmartContract subscribes to the MarketEvent published by the MarketWatcher Cryptlet.  In the subscription it tells the MarketEvent when to wake it up, only if the markets were open that day and what prices it wants when the event is fired.  The MarketWatcher Cryptlet will then wake the SmartContract up with a message at exactly 4:00 PM EST, if the markets were open that day and provide the LIBOR rate and the price of gold in the message.  The message is routed directly to the call back function CalculatePrice() which will then perform the logic with the data provided. 

###Control

Control is the pattern that Contract Cryptlets, described in another section of this paper, follow.  SmartContracts that use Control will delegate all of the business logic that needs to be performed to the Cryptlet.  The SmartContract inherits from the ContractCryptlet class that wires up the signature and meta data required for functionality and implements the data or state with a variable + property accessor to store state.  
 

![<Control UC>](<images/ControlUC.png>)

In this example, the SmartContract is basically defining a state table, accessor properties to update the data with messages sent by the Control Cryptlet.  This pattern allows for the logic within a SmartContract to be run on a defined number of compute resources (at least 1) and located, scaled and secured independently from the blockchain itself.  

For example, Control would let you run your logic on a massive VM instance with a lot of RAM and solid state storage that is co-located within an exchange for optimum performance and private execution.

## Common Characteristics
Cryptlets must go through a registration process in order to setup their environment for execution and dependent secure infrastructure.  There are several ways this can be done depending on the Cryptlet type.

Cryptlet header files are exported as a part of the registration package for Cryptlets.  The header contains the interface definition, custom types as well as the public key property for the Cryptlet.

```csharp
	//Utility Cryptlet
	Contract PropertyPrivacyCryptlet{
    	address cryptlet = 0x39e2842....;
    	function();
    	event();
	}

	//Contract Cryptlet base type
	Contract ContractCryptlet{
    	uint minimumInstances;
    	uint maximumInstances;
    	address cryptlet;
    	function();
    	event();
    	// check that only the Cryptlet surrogate
   		// is able to call the SmartContract
    	modifier onlyBy(address _cryptlet)
    	{
        	if (msg.sender != _cryptlet)
           	 throw;
		}
	...
	}
```
Cryptlet's configuration uses standard JSON formatting as extensions to the Cryptlet JSON schema:
```
{
    "title": "Cryptlet Schema",
    "type": "object",
    "properties": {
        "name": {
            "type": "string"
        },
        "publicKey": {
            "type": "string"
        },
        "config": {
            "description": "describe what Cryptlet does",
            "isolation": "boolean",
            "...": 
        }
    },
    "required": ["name", "publicKey"]
}
```
Cryptlets get registered with the CryptletHostContainerService, the Cryptlet is tokenized and assigned its public/private key pair, the JSON configuration file and header are recorded to the CryptletRegistrationBlockchain.  Cryptlet Registrar is a blockchain database maintained by the CryptletHostContainerService.  Cryptlet identities and meta data are stored in the blockchain and linked to distributed data stores for meta data and policy.  Utility Cryptlets are registered independently from SmartContracts and are referenced by them, where Contract Cryptlets are registered at runtime when the SmartContract is deployed to the blockchain or the Control Cryptlet Deployment tool in the Bletchley SDK.

![<Utility Cryptlet>](<images/utilityCryptlet.png>)


![<Contract Cryptlet>](<images/contractCryptlet.png>)


At design time, developers make references to Cryptlets they intend to use recording the references and public keys of these Cryptlets into their code.  For example, the Cryptlet interface in the SmartContract being imported:
```javascript

import "github.com/cryptlets/encryption/propertyPrivacy.sol" as propertyPrivacy;
import "github.com/cryptlets/stocktools/equitytools" as equityTools;

```
Using Ethereum contract’s init() or constructor functions, Cryptlets are assigned an attested host and their reference is stored in the SmartContract and event subscriptions between Cryptlets and the SmartContract can be established.  SmartContracts using cryptlets can derive from the CryptletProxy base class that will provide most of the default configuration.   SmartContracts that will delegate their runtime logic to a Contract Cryptlet will inherit from ContractCryptlet base class.  The CryptoDelegate will work against the base classes to intercept delegation requests and perform security checks.  

```csharp
//using a Utility Cryptlet
Contract CryptletProxy{
	function cryptletProxy() {owner = msg.sender;}
	address owner;
	string name;
	address publicKey;
	string attestedHost;
	string interface;
}

contract PropertyPrivacy is CryptletProxy{
	name = propertyPrivacyCryptlet;
	publicKey =’0xd5f9d8d94886e70b06e474c3fb14fd43e2f23970’;
	attestedHost = ‘19 3E EA B7 C5 54 60 1D 81 BD…’;
	interface = propertyPrivacy; //import statement
};
contract StockClient is CryptletProxy{
	name = stockClientCryptlet;
	publicKey =’0xe7f9d8d94886e70b06e474c3fb14fd43e2f2381’;
	attestedHost = ‘19 3E EA B7 C5 54 60 1D 81 BD…’;
	interface = equityTools; //import statement
};
```
Wiring up event callback functions with attributes and creating proxies for calling Cryptlet static methods, optionally adding access control entries for addresses that are able to respond or call back is performed by the CryptoDelegate.

```csharp
var stockClient;
function init(){ //or function smartContractName() as the constructor
	stockClient = StockClient();
	[event(stockclient.PriceUpdate.Subscribe(‘au’, CalculatePrice)];
}
//callback method that is run when the Cryptlet PriceUpdate event fires
function CalculatePrice(uint price){ ... }
```
Cryptlet behaviors like process isolation using enclaving to run the CryptletContainer and Cryptlet in an isolated enclave can be set in code.  Process isolation can also be set with policy in the CryptletHostContainerService that should always override code settings.

`PropertyPrivacy.processIsolation = true;`

Attributes can be placed in the code indicating that execution of logic at the attribution point is delegated through the CryptoDelegate.  For example, if a property in a SmartContract needs to be encrypted and only visible to counter-parties of the contract, an attribute like this can be applied.
```csharp
[encryptField='ContractSignersOnly']
uint public trade_amount = 0;
```
A Contract Cryptlet will be declared in the SmartContract itself and can be installed during deployment to the blockchain via the SmartContract constructor.  The SmartContract using full delegation of processing to the Cryptlet will simply store the authentic values and signatures instead of running the logic.  This will allow Contract Cryptlets to be more dynamic and can support scale up models where more performance is required and parallel execution is desired.    
```csharp
Contract CreditDefaultSwap is ContractCryptlet{
	uint price;
		uint rate;
		//constructor definition of Contract Cryptlet
```
SmartContracts that use Contract Cryptlets will need to provide property accessors for values as functions allowing the Cryptlet to update its state on the blockchain.  Inheriting from ContractCryptletBase will provide the default accessors following naming conventions as well as snapshot state functions.  These can be overridden for each variable or as a whole with a ContractStateSnapshot.
```csharp
Contract CreditDefaultSwapStateSnapshot{
	uint price;
	uint quantity;
}
...
	function UpdateState(CreditDefaultSwapStateSnapshot state){
	...
}
```
When the SmartContract below is installed into the blockchain the constructor is called.  Because it inherits from ContractCryptlet, its constructor will be passed to the CryptoDelegate for the Contract Cryptlet to be created.  The CryptoDelegate passes the configuration and parameters for the Contract Cryptlet to the CryptletHostContainerService for creation.  
```csharp
//SmartContract using Contract Cryptlet constructor
     function CreditDefaultSwap(){
			minimumInstances = 1;
			maximumInstances = 5;
			//code could be brought in with an include statement
			CryptletCode code = "c#, java, C++, etc. code ...";
        }
	}
```
The CryptletHostContainerService will create the Cryptlet based on the information and code passed in to setup and deploy the Cryptlet into the cloud.  For example, the language/platform: Java, C#, etc. code for the Cryptlet are passed and validated for the Contract Cryptlet to be created, installed and run in the cloud.

Cryptlet functionality will be exposed and accessed through Aspects introduced into Solidity.  These aspects will usually be expressed with attributes.  For example, a SmartContract can specify that a field is to be encrypted, but not how.  That is determined at runtime based on policy.  At node startup, a new command line option will specify where the CryptoDelegate looks up Cryptlet Registration and Policies.
```bash
geth --cryptoDelegatePath 'https://azure.com/myconsortium' --cryptoDelegateSignature '0x231dw...'
```

![<Secure Communications>](<images/secureCommunications.png>)

Additionally, Cryptlets and their CryptletContainer could be signed by or include digital signatures from identities that would allow a Cryptlet to do work “on-behalf of” one or more identities.  For example, a user could create a Cryptlet and sign it with their digital signature and when invoked would perform actions as an agent for the user in a business process.  Interesting scenarios can come out of Cryptlets that are signed at design time and those that could potentially be enlisted and signed at runtime to work on behalf of scenarios.

## Cryptlet Development
Cryptlets can be created using the Bletchley SDK.  A developer will need to select one of the languages supported and type desired, which will include all of the references and settings for the CryptletContainer.  For example, a developer can choose to create a Cryptlet in C# without enclaving, which would set the base CryptletContainer to the .NET CORE CryptletContainer with enclaving disabled.

The developer then inherits from either the UtilityCryptlet or ContractCryptlet base class, builds the project and deploys to a test or production environment.

```csharp
public class CustomerValidatorCryptlet as UtilityCryptlet
{
...
}

```

## Cryptlet Registration
Cryptlets are registered with the CryptletHostContainerService which maintains the CryptletRegistrationBlockchain and linked distributed storage.  Registration of a Cryptlet can be done either with the CryptletRegistrationCLI, Deployment Wizard or using the CryptoDelegate for Contract Cryptlets.

![<Cryptlet Registration>](<images/cryptletRegistration.png>)

Depending on the creation point, the private keys for each Cryptlet are stored in the Azure Key Vault by their Registration Blockchain address to be retrieved when the Cryptlet requires it.  The CryptletContainer can create the keypair for Contract Cryptlets while in an enclave and store the keys in the KeyVault by their Internal Address.

## Cryptlet Container
CryptletContainers will host a runtime VM environment to support different platforms like Java, C# and perhaps functional languages and is responsible for bootstrapping Cryptlets, managing Cryptlet lifecycle and I/O from the enclave.  CryptletContainers are wrapped around Cryptlets as packages for deployment by either the CryptoDelegate creating a new Contract Cryptlet using the CryptletHostContainerService or by the Cryptlet deployment tool. Initial target runtimes include .NET Core and the JVM which are the most popular in the enterprise.

![<Cryptlet Containers>](<images/cryptletContainers.png>)

CryptletContainer provides access to keys for the hosted Cryptlet from the Azure Key Vault, or if using an enclave, a sealed state. Also it provides the I/O interfaces for the Cryptlet to communicate with other enclaves and unsecure processes.  CryptletContainers instantiate Cryptlets providing them their identification and access to private keys for encryption and signing.

```csharp
class CryptletContainer {
	static KeyVault kv = KeyVault(….);
	CryptletId cryptletId;
CryptletKeys[] cryptletKeys;
	
	
public CryptletContainer(bool enclaved){
		if (enclaved){
			cryptletKeys = kv.getKeys(cryptletId);			
		}
		else{
			cryptletKeys = cryptletId.packageKeys;
		}
		
	}
	
	init(){
		if (!kv.exists(cryptletId){
			//generate new key pair for new Contract Cryptlet
		}
	}
}
```
## Enclaving
Use of secure computation via enclaves (isolation of code within a secure boundary) implemented via hardware using hardware or Hypervisor-based solutions provides Cryptlets the environment to perform isolated execution and encryption services.  Building off of Microsoft Researchers work on Secure Computation Interfaces the CryptletHostContainerService, CryptoDelegate and CryptletContainer can implement these interfaces to setup a secure, private operating environment while abstracting away the actual enclaving technology being used.

When the CryptletHostContainerService receives the first call for a Cryptlet that needs to be isolated, requires encryption/decryption capabilities or is a Contract Cryptlet, a secure isolated region will need to be created by the CryptletHostContainerService:
```cpp
HANDLE
IsolatedRegionCreate(
     _In_     LPCTSTR            packagePath, //this is the CryptletContainer
 package
     _In_     ISOLATION_PROVIDER isolationProvider,
     _In_opt_ CALL_OUT_HANDLER   callOutHandler     
) 

```
The CryptletContainer is created in a new enclave and implements the interface to communicate back the untrusted code (CryptletHostContainerService or CryptoDelegate) like:

```cpp
IRIO_RESULT 
IsolatedRegionIoControl(     
_In_      HANDLE  region,
_In_      DWORD   callInId,

_In_reads_bytes_opt_(inputBufferBytes)
     LPCVOID inputBuffer,
 _In_      SIZE_T  inputBufferBytes,
_Out_writes_bytes_to_opt_(outputBufferBytes, *bytesReturned)
               LPVOID  outputBuffer,     
_In_      SIZE_T  outputBufferBytes,
_Out_opt_ PSIZE_T bytesReturned     
)
```

The CryptletContainer package identifies the function that handles the control code passed in and all I/O within the Cryptlet.
```cpp
IRIO-RESULT
IsolatedAppIoControl(
    _In_        DWORD   callOutId,
    _In_reads_bytes_opt_(inputBufferBytes) LPCVOID inputBuffer,
    _In_        SIZE_T  inputBufferBytes,
    _Out_writes_bytes_to_opt_(outputBufferBytes, *bytesReturned)
         LPVOID  outputBuffer,
    _In_        SIZE_T  outputBufferBytes,
    _Out_opt_ PSIZE_T bytesReturned
)
```
Cryptlets in the trusted environment can get authentication codes or signatures for data to send out from their CryptletContainer:
```cpp
BOOL
IsolatedAppSignMessage(
_In_reads_bytes_(messageBytes) LPCVOID message,
_In_ SIZE_T messageBytes,
_Out_writes_bytes_to_(outputBufferBytes, *outputBufferBytesRequired)
         LPVOID outputBuffer,
_In_ SIZE_T outputBufferBytes,
_Always_(_Out_) PSIZE_T outputBufferBytesRequired
)
```

This function generates the signature for the buffer for output (I/O) with the private keys only available to the provider or enclave and validated using public keys from the provider.

These keys also allow for the encryption of data to be saved to the blockchain or to decrypt it from the blockchain.  Keys for encryption can be fetched from the Key Vault for schemes like Threshold, Ring, etc.

The untrusted code, CryptoDelegate or CryptletHostContainerService or another enclaved Cryptlet can verify that the message originated from the enclaved Cryptlet with these functions:
```cpp
BOOL 
IsolatedRegionGetDigest(
     _In_ LPCTSTR packagePath,
     _Out_writes_bytes_to_(regionDigestBytes, *regionDigestBytesRequired)
          LPVOID regionDigest,
     _In_ SIZE_T regionDigestBytes,
     _Out_ PSIZE_T regionDigestBytesRequired
     ) 

BOOL 
IsolatedRegionCheckSignature(
     _In_     ISOLATION_PROVIDER isolationProvider,
     _In_reads_(regionDigestBytes) LPCVOID regionDigest,
     _In_ SIZE_T regionDigestBytes,
     _In_reads_(messageBytes) LPCVOID message,
     _In_ SIZE_T messageBytes,
     _In_reads_(signatureBytes) LPCVOID signature,
     _In_ SIZE_T signatureBytes
     )
```

IsolatedRegionGetDigest returns a digest that identifies the Cryptlet, which is then passed to the IsolatedRegionCheckSignature function along with the identifier for the enclave provider (public key).  The function returns true if the message was produced by the Cryptlet in the enclave.  The contents of the signed message establish the trust between the Cryptlet Fabric and the CryptoDelegate or CryptletHostContainerService using different key exchanges like Diffie-Hellman, etc.

When a Contract Cryptlet is created via the CryptoDelegate, the CryptletHostContainerService will create a new enclave providing the CryptletContainer package to bootstrap.  The CryptletContainer packages init() function will generate a new key pair within the enclave and store the private key in the Azure Key Vault and pass the public key into the constructor for the Cryptlet.
```csharp
Init(){
	var keypair = new keypair(….);
	KeyVault.Store(keypair.privateKey);
	Cryptlet theCryptlet = new Cryptlet(keypair.publicKey);
}
```
This will bootstrap the new Contract Cryptlet into its enclave and it can begin executing signing its communications back to the SmartContract securely.  Utility Cryptlets requiring enclaving, encryption or privacy requirements, should perform this step during installation.

If a Cryptlet is multi-instance or needs re-instantiation, the CryptletHostContainerService will create a new enclave and in the constructor of the CrypletContainer, fetch the key pair from the Azure Key Vault, then instantiating the Cryptlet as above. Cryptlets can be re-instantiated from a secured enclave image.

```csharp
...
	CryptletContainer newContainer = new 
		CryptletContainer(KeyVault.getKeyPair(package.cryptletId));
...
```
![<Cryptlet Bootstrap>](<images/cryptletBootstrap.png>)

Enclaved Cryptlets can fetch and store keys in the Azure Key Vault for various purposes.  The CryptletContainer provides the interfaces for communicating to the KeyVault and other Cryptlets that are in separate enclaves.

## Conclusion
The addition of a blockchain Cryptlet Fabric tier can be described as Blockchain 3.0: Data and logic on a blockchain with Cryptlets called via CryptoDelegates from a Smart Contract for off-chain functionality forming a three-tiered architecture.

![<EVOLVE>](<images/evolution.png>)

Creating a formal security model for interacting with the real world and providing interoperability with existing systems in a trusted way will help adoption within enterprise consortiums and provide an additional level of re-use for developers writing cryptlets.  It also opens up an entire Cryptlet Fabric tier for blockchains of any sort and provides a flexible execution environment allowing for partial delegation of logic and full delegation for greater scale.

![<BeltchleyEcosystem>](<images/bletchleyEco.png>)

Cryptlets will provide the infrastructure required to deliver a suite of blockchain functionality in the middle tier as libraries or services initially identified as: Blockchain Gateway Services, Identity & Key Management, Privacy & Encryption as well as Data Services (machine learning, analytics and BI Dashboards).

##Appendix
Components and Actors
* Cryptlet - A small program written to run within a CryptletContainer that executes programs to provide secure services and data to blockchains.

* CryptletContainer - A container runtime that consists of a shell that can optionally run within a secure enclave.  Each CryptletContainer has the interfaces to support Cryptlet services, I/O, Key Services, etc.  Additionally, it contains a runtime for Cryptlets to run.  Initially .NET Core and the JVM are supported with other runtimes based on demand.

* Enclave - An isolated region for code and data to function usually on silicon that secured by the CPU.  Enclaves are tamper proof areas that make it possible to remotely attest to the integrity of the code and data within the enclave and trust its results.  

* CryptletHostContainerService - Azure Service that listens for CryptoDelegate or CryptletAdapter requests for Cryptlet services and instantiates or routes requests appropriately.  This service uses the explicit dependencies principle supporting a modular and extensible framework to support multiple blockchain communication patterns.  Uses CryptletRegister to configuration and Cryptlet policies. 

* CryptletRegister - Internal service configuration database for Cryptlets and related services.  Allows policy to be set by administrators.  Register is a mix of data stores including distributed file, blockchain, etc.

* CryptoDelegate - Modular extension of blockchain virtual machine (ex. Ethereum Virtual Machine) that intercepts Cryptlet communications, checks signatures, provides deployment services, wires secure communications between the SmartContract (host node) and the CrypletFabric.  A Cryptlet Adapter provides similar but less integrated functionality for non-SmartContract based platforms like UTXO Opcode based platforms.

* CryptletAdaptor - is it spelled adapter or adaptor? also needs a description

* CrypletFabric - needs a description 
    
_____
*This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.*
