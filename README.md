# Gaming SDK 🎮 

<p>Welcome to the documentation of the Vottun Gaming SDK! This powerful SDK enables web3 login, IPFS file uploads, and NFT operations (minting, transfers, metadata retrieval, etc.) for seamless integration into your Unity projects. Enhance your gaming experience with secure and decentralized blockchain functionalities. Let's get started!</p>

<br/>
<br/>

## How it works 

The SDK implements the MetaMask Unity SDK to render a QR that the player can scan with his MetaMask wallet to establish connection with the game, make sure the user’s wallet is on a certain network, and get the public address of the user’s wallet.  

Once the Vottun SDK has the public address, it will automatically proceed with the Web3 Login standard and will send a message to the user’s wallet with the objective of verifying that it is a real wallet and not any type of impersonation.  

When all the process is done, the user will be capable of using all the Gaming Api functionalities of your game.

<br/>
<br/>

## Steps 

### 1. Import SDK 

Your first step is to import the Vottun SDK as any other .unitypackage

Once it is imported, you will notice a new tab on the top of the screen called “Tools”, you will have to navigate to **Tools -> VottunSDK -> Install**.

A popup window will appear, it is the package installer window, where you will install all the dependencies needed by the MetaMask SDK and the Vottun SDK. Click “install” on both dependencies, Newtonsoft.Json and JAR Resolver, once they are installed click the Install VottunSDK button and the installation of the main package will begin.


> :warning: ***The following error may occur, but you can ignore it:***
```
Assembly 'Assets/ExternalDependencyManager/Editor/1.2.175/Google.IOSResolver.dll' will not be loaded due to errors:  
Unable to resolve reference 'UnityEditor.iOS.Extensions.Xcode'. Is the assembly missing or incompatible with the current platform?  
Reference validation can be disabled in the Plugin Inspector.
```

<br/>

### 2. Configure Vottun SDK and MetaMask SDK 

The next step is to configure the MetaMask SDK so when the player connects his wallet the information shown in the MetaMask app will match your project.

On the tabs navigate to **Window -> MetaMask -> Setup Window** and the MetaMask Setup popup window will appear, go to “credentials” and change the “App Name” and “App URL” fields to the name and url of your project, next uncheck the “Deep Linking” option and click on “Apply Settings”, once you are done you can close the MetaMask Setup window.


> :warning: ***This is the basic configuration to work properly with the Vottun SDK, you can configure the MetaMask SDK as you want but may create problems on the compatibility between Vottun SDK and MetaMask SDK.***

Now, on the project folders, navigate to **Assets -> VottunSDK -> VottunUnity -> VottunSdkConfig -> Resources**. Here you will find three scriptable objects used for configuration of the Vottun SDK.

The first scriptable object is called “Gaming Api Config”, and it's where you will write the JWT code given by Vottun when you buy the Gaming API service, to get access to the API, and the URL of the gaming API, so if the URL changes in the future it’s as easy as changing the URL here, and you will not have to update the package if you don’t want to.
- **Gaming JWT:** The JWT token given by Vottun
- **Gaming Url:** The URL of the Gaming Api


> :warning: ***The Gaming API URL at this moment is: https://intapi.vottun.tech/game/v1
If it changes in the future, this message will be updated with the new URL***


The second scriptable object is called “Sign Message Config”, and it’s where you will configure the message that will show on the player’s wallet when the Web3 login process occurs, the player will use this message to confirm that the connection is secure and not any kind of scam or impersonation, so the message has to be clear and explain the use of all of the player’s information.
- **Domain:** The domain of your project.
- **Chain ID:** The ID, in numerical format, of the blockchain network that your projects use.
- **Statement:** The message that will appear in the player’s wallet to sign.
- **Uri:** The URL of your project

The third scriptable object is called “Wallet Connection Config”, and it’s where you will configure the network where the player’s wallet will have to be so it can connect to your project.

- **Force Network:** If you want to force the player to stay on the configured network.
- **Network ID:** The ID, in hexadecimal format, of the network where your project is.
- **Network Name:** The name of the network where your project is.
- **Currency Name:** The name of the currency of the network where your project is.
- **Currency Symbol:** The symbol of the currency of the network where your project is.
- **Currency Decimals:** The number of decimals of the currency of the network where your project is.
- **Network Rpc Urls:** An string array, where each string is a URL to an RPC of the network where your project is.
- **Network Block Explorer Urls:** An string array, where each string is a URL to a blockexplorer of the network where your project is.
- **Network Icon Urls:** An string array, where each string is a URL to an icon of the network where your project is.


> :warning: ***The default configuration when you install the SDK is valid for the Avalanche Fuji Testnet***


> :warning: ***The Network Icon Urls field is optional and you can leave it as an empty array if you want, for all the other fields, you can find the needed information on websites like https://chainlist.org***

<br/>

### 3. Scene Setup 

Now it’s time to set up the scene where the Player will connect his wallet to your project.

You will need a Canvas to display the QR, you can customize it as you want, you can use one that already exists or create a new canvas, that’s totally on to you.

Then, on the project assets, you will navigate to **Assets -> VottunSDK** and add the prefab called QRImage as a child of your Canvas. This is a Raw Image that will display the QR that the player needs to scan to connect his wallet. You can scale it or customize it as you want.

Once you have the QRImage where you want to, navigate again to **Assets -> VottunSDK** and add the prefab called VottunSDK to your scene. This prefab is an Empty Gameobject with 4 Empty Gameobjects more inside, that will handle all the logic to establish the connection between your project and the player’s wallet.
- **Wallet Connection Manager:** Handle all the connection process and Web3 Login process with the player’s wallet. You can disable or enable console logs of the connection process here The function “Show QR” displays the QR if the Initialize on Start of “Meta Mask Unity” is disabled.
- **Meta Mask Unity:** The main script of the MetaMask SDK, here you can select if initialize the MetaMask SDK on the Start of the scene or initialize it manually.
- **Meta Mask Unity Transport Broadcaster:** Loads the QR on the listener assigned to it.
- **Wallet Event Manager:** Centralize all the Events of the connection of the MetaMask SDK so you will be able to subscribe to all the events in a simpler way than with only the MetaMask SDK.


> :warning: ***The event “addressAvailable” has the wallet address on its args, you must subscribe to this event in order to get the player’s wallet address.***

The last step of the setup is to go to the “Meta Mask Unity Transport Broadcaster” and add the QRImage prefab of the scene to its “Listeners” array.

<br/>

### 4. Use the API 

Now you have all the configurations needed done, and it’s time to start using all the benefits of blockchain on your project.

All the functions of the API are methods in the GamingApi class (**Assets -> VottunSDK -> VottunUnity -> GamingApi**). You must instantiate an object of this class in your own script where you want to use the Gaming API, then call the method of the desired endpoint, passing all the parameters that the method needs.

<br/>
<br/>

## SDK methods 

Let’s assume that the address of the player’s wallet is stored in a static class called “playerData” and that all the methods are called in a script name **ApiHandler.cs** that will only handle the calls to the API.

The start of the script will look like this:

```cs title="ApiHandler.cs"
public class ApiHandler
{
	private string walletAddress;
	private GamingApi gamingApi;

    private void Start()
    {
        walletAddress = playerData.walletAddress;
        gamingApi = new GamingApi();
    }
}
```

Now that we have the base of the script defined, let's see how we can implement each method of the GamingAPI class.

<br/>

### Balance Of 

With this method, you will be able to know how many of a specific NFT owns the player.

To implement it, you should create an **IEnumerator** which receives:
- **Contract:**   
    - **Type:** String
    - **Description:** The ID of the contract of the NFT
- **Network:**
    - **Type:** String
    - **Description:** The ID of the network, in **decimal format**, of the NFT
- **Address:**
    - **Type:** String
    - **Description:** The wallet address of the wallet where you want to see the balance of the NFT
- **Id:**
    - **Type:** String
    - **Description:** The ID of the NFT to get his balance

Inside this IEnumerator you should create a variable of type **int**. Here is where the **balance** will get stored once the API returns the ammount of NFTs.

Then you should start the coroutine **"BalanceOf"** inside the **"gamingApi"** class, with the **yield return** statement, and with a **delegate** that will set the balance that the API returns as the value of the previously created variable, and all the values recived on the IEnumerator, in the following order:
1. Contract
2. Network
3. Addres
4. Id 

Here is an example of how it should be implemented to print on the console the **ammount** of a specific NFT that the player has in his wallet:

```cs title=apiHandler.cs
private void callGetBalance()
{
    StartCoroutine(BalanceOf("contractAddress", "networkId", walletAddress, "nftId"));
}

IEnumerator BalanceOf(string contract, string network, string address, string id)
{
    int resultBalance = 0;
    
    yield return StartCoroutine(gamingApi.BalanceOfNft(balance => resultBalance = balance, contract, network, address, id));

    OnBalanceRecived(resultBalance);
}

private void OnBalanceRecived(int balance)
{
    // Apply your desired logic here
    Debug.Log($"The player has {balance} of this NFT");
}
```

<br/>

### Token URI 

With this method, you will be able to get the URI of a specific NFT.  

To implement it, you should create an **IEnumerator** which receives:
- **Contract:**   
    - **Type:** String
    - **Description:** The ID of the contract of the NFT
- **Network:**
    - **Type:** String
    - **Description:** The ID of the network, in **decimal format**, of the NFT
- **Id:**
    - **Type:** String
    - **Description:** The ID of the NFT to get his balance

Inside this IEnumerator you should create a variable of type **string**. Here is where the **URI** of the NFT will get stored once the API returns the URI.

Then you should start the coroutine **"TokenUri"** inside the **"gamingApi"** class, with the **yield return** statement, and with a **delegate** that will set the URI that the API returns as the value of the previously created variable, and all the values recived on the IEnumerator, in the following order:
1. Contract
2. Network
3. Id

Here is an example of how it should be implemented to print on the console the **URI** of a specific NFT:

```cs title=apiHandler.cs
private void callTokenUri()
{
    StartCoroutine(TokenUri("contractAddress", "networkId", "nftId"));
}

IEnumerator TokenUri(string contract, string network, string id)
{
    string resultUri = "";

    yield return StartCoroutine(gamingApi.TokenUri(uri => resultUri = uri, contract, network, id));

    OnTokenUriRecived(resultUri);    
}

private void OnTokenUriRecived(string uri)
{
    // Apply your desired logic here
    Debug.Log(uri);
}
```

<br/>

### Mint 

With this method, you will be able to mint one NFT.  

To implement it, you should create an **IEnumerator** which receives:
- **Contract:**   
    - **Type:** String
    - **Description:** The ID of the contract of the NFT
- **Network:**
    - **Type:** Int
    - **Description:** The ID of the network, in **decimal format**, of the NFT
- **WalletAddress:**
    - **Type:** String
    - **Description:** The address of the wallet where the NFT will be minted
- **Amount:**
    - **Type:** Int
    - **Description:** The ammount of copies of the NFT that will be minted
- **Uri:**
    - **Type:** String
    - **Description:** The URI of the metadata of the NFT

Inside this IEnumerator you should create two variables of type **string**. Here is where the **transaction hash** and the **transaction nonce ** of the mint operation will get stored once the API mint the NFT.

Then you should start the coroutine **"Mint"** inside the **"gamingApi"** class, with the **yield return** statement, and with two **delegates** that will set the **transaction hash** and the **transaction nonce** that the API returns as the values of the previously created variables, and all the values recived on the IEnumerator, in the following order:
1. Contract
2. Network
3. WalletAddress
4. Amount
5. Uri

Here is an example of how it should be implemented to mint the NFT on the player's wallet and print on the console the **transaction hash** and the **transaction nonce** of the mint operation once is completed:

```cs title=apiHandler.cs
private void CallMint()
{
    StartCoroutine(Mint("contractAddress", 0, "walletAddress", 5, "nftUri"));
}

IEnumerator Mint(string contract, int network, string walletAddress, int amount, string uri)
{
    string responseTxHash = "";
    string responseNonce = "";

    yield return StartCoroutine(gamingApi.Mint(t => responseTxHash = t, n => responseNonce = n, contract, network, walletAddress, amount, uri));

    OnNftMinted(responseTxHash, responseNonce);
}

private void OnNftMinted(string txHash, string nonce)
{
    // Apply your desired logic here
    Debug.Log($"ID: {txHash} - Network: {nonce}");
}
```

<br/>

### Mint Batch 

With this method, you will be able to mint multiple NFTs at once.  

To implement it, you should create an **IEnumerator** which receives:
- **Contract:**   
    - **Type:** String
    - **Description:** The ID of the contract of the NFTs
- **Network:**
    - **Type:** Int
    - **Description:** The ID of the network, in **decimal format**, of the NFTs
- **WalletAddress:**
    - **Type:** String
    - **Description:** The address of the wallet where the NFTs will be minted
- **Amounts:**
    - **Type:** Int Array
    - **Description:** Array with the ammounts of copies of each NFT that will be minted
- **Uris:**
    - **Type:** String Array
    - **Description:** Array with the URIs of the metadata of each NFT

Inside this IEnumerator you should create two variables of type **string**. Here is where the **transaction hash** and the **transaction nonce ** of the mint operation will get stored once the API mint all the NFTs.

Then you should start the coroutine **"MintBatch"** inside the **"gamingApi"** class, with the **yield return** statement, and with two **delegates** that will set the **transaction hash** and the **transaction nonce** that the API returns as the values of the previously created variables, and all the values recived on the IEnumerator, in the following order:
1. Contract
2. Network
3. WalletAddress
4. Amounts
5. Uris

Here is an example of how it should be implemented to mint the NFTs on the player's wallet and print on the console the **transaction hash** and the **transaction nonce** of the mint operation once is completed:

```cs title=apiHandler.cs
private void CallMintBatch()
{
    int[] ammounts = {3, 5};
    string[] uris = {"firstNftUri", "secondNftUri};
    StartCoroutine(MintBatch("contractAddress", 0, walletAddress, ammounts, uris));
}

IEnumerator MintBatch(string contract, int network, string walletAddress, int[] amounts, string[] uris)
{
    string responseTxHash = "";
    string responseNonce = "";

    yield return StartCoroutine(gamingApi.MintBatch(t => responseTxHash = t, n => responseNonce = n, contract, network, walletAddress, amounts, uris));

    OnMultipleNftsMinted(responseTxHash, responseNonce);
}

private void OnMultipleNftsMinted(string txHash, string nonce)
{
    // Apply your desired logic here
    Debug.Log($"ID: {txHash} - Network: {nonce}");
}
```

<br/>

### Transfer 

With this method, you will be able to transfer one NFT from one wallet to another.  

To implement it, you should create an **IEnumerator** which receives:
- **Contract:**   
    - **Type:** String
    - **Description:** The ID of the contract of the NFT
- **Network:**
    - **Type:** Int
    - **Description:** The ID of the network, in **decimal format**, of the NFT
- **OriginWalletAddres:**
    - **Type:** String
    - **Description:** The address of the wallet from where the NFT will be transferred  
- **DestinationWalletAddress:**
    - **Type:** String
    - **Description:** The address of the wallet where the NFT will be transferred
- **Id:**
    - **Type:** Int
    - **Description:** The ID of the NFT that will be transferred
- **Amount:**
    - **Type:** Int
    - **Description:** The amount of the smae NFT that will be transferred

Inside this IEnumerator you should create two variables of type **string**. Here is where the **transaction hash** and the **transaction nonce ** of the transfer operation will get stored once the API transfers the NFT.

Then you should start the coroutine **"Transfer"** inside the **"gamingApi"** class, with the **yield return** statement, and with two **delegates** that will set the **transaction hash** and the **transaction nonce** that the API returns as the values of the previously created variables, and all the values recived on the IEnumerator, in the following order:
1. Contract
2. Network
3. OriginWalletAddress
4. DestinationWalletAddress
5. Id
6. Ammount

Here is an example of how it should be implemented to transfer 5 copies of an NFT from the project wallet to the player's wallet and print on the console the **transaction hash** and the **transaction nonce** of the transfer operation once is completed:

```cs title=apiHandler.cs
private void CallTransfer()
{
    int[] ammounts = {3, 5};
    string[] uris = {"firstNftUri", "secondNftUri};
    StartCoroutine(Transfer("contractAddress", 0, "projectWalletAddress", walletAddress, 0, 5));
}

IEnumerator Transfer(string contract, int network, string originWalletAddress, string destinationWalletAddress, int id, int amount)
{
    string responseTxHash = "";
    string responseNonce = "";

    yield return StartCoroutine(gamingApi.Transfer(i => responseTxHash = i, n => responseNonce = n, contract, network, originWalletAddress, destinationWalletAddress, id, amount));

    OnNftTransferred(responseTxHash, responseNonce)
}

private void OnNftTransferred(string txHash, string nonce)
{
    // Apply your desired logic here
    Debug.Log($"ID: {txHash} - Network: {nonce}");
}
```

<br/>

### Transfer Batch 

With this method, you will be able to transfer multiple NFTs from one wallet to another.  

To implement it, you should create an **IEnumerator** which receives:
- **Contract:**   
    - **Type:** String
    - **Description:** The ID of the contract of the NFT
- **Network:**
    - **Type:** Int
    - **Description:** The ID of the network, in **decimal format**, of the NFT
- **OriginWalletAddres:**
    - **Type:** String
    - **Description:** The address of the wallet from where the NFT will be transferred  
- **DestinationWalletAddress:**
    - **Type:** String
    - **Description:** The address of the wallet where the NFT will be transferred
- **Ids:**
    - **Type:** Int Array
    - **Description:** The IDs of the NFTs that will be transferred
- **Amounts:**
    - **Type:** Int Array
    - **Description:** The amounts of the NFTs that will be transferred

Inside this IEnumerator you should create two variables of type **string**. Here is where the **transaction hash** and the **transaction nonce ** of the transfer batch operation will get stored once the API transfers the NFTs.

Then you should start the coroutine **"TransferBatch"** inside the **"gamingApi"** class, with the **yield return** statement, and with two **delegates** that will set the **transaction hash** and the **transaction nonce** that the API returns as the values of the previously created variables, and all the values recived on the IEnumerator, in the following order:
1. Contract
2. Network
3. OriginWalletAddress
4. DestinationWalletAddress
5. Ids
6. Ammounts

Here is an example of how it should be implemented to transfer 3 copies of one NFT and 5 copies of another NFT from the project wallet to the player's wallet and print on the console the **transaction hash** and the **transaction nonce** of the transfer batch operation once is completed:

```cs title=apiHandler.cs
private void CallTransferBatch()
{
    int[] ids = {0, 1};
    string[] amounts = {3, 5};
    StartCoroutine(TransferBatch("contractAddress", 0, "projectWalletAddress", walletAddress, ids, ammounts));
}

IEnumerator TransferBatch(string contract, int network, string originWalletAddress, string destinationWalletAddress, int[] ids, int[] amounts)
{
    string responseTxHash = "";
    string responseNonce = "";

    yield return StartCoroutine(gamingApi.TransferBatch(i => responseTxHash = i, n => responseNonce = n, contract, network, originWalletAddress, destinationWalletAddress, ids, amounts));

    OnMultipleNftsTransferred(responseTxHash, responseNonce);
}

private void OnMultipleNftsTransferred(string txHash, string nonce)
{
    // Apply your desired logic here
    Debug.Log($"ID: {txHash} - Network: {nonce}");
}
```

<br/>

### Upload File 

With this method, you will be able to upload a file to use it as NFT.  

To implement it, you should create an **IEnumerator** which receives:
- **Path:**   
    - **Type:** String
    - **Description:** The path to the file to upload
- **Filename:**
    - **Type:** String
    - **Description:** The name of the file to upload

Inside this IEnumerator you should create a variable of type **string**. Here is where the **hash** of the upload operation will get stored once the API uploads the file.

Then you should start the coroutine **"UploadFile"** inside the **"gamingApi"** class, with the **yield return** statement, and with a **delegate** that will set the hash that the API returns as the value of the previously created variable, and all the values recived on the IEnumerator, in the following order:
1. Path
2. Filename

Here is an example of how it should be implemented to upload a file and print on the console the **hash** of the upload operation once is completed:

```cs title=apiHandler.cs
private void CallUploadFile()
{
    StartCoroutine(UploadFile("filePath", "fileName"));
}

IEnumerator UploadFile(string path, string filename)
{
    string resultHash = "";

    yield return StartCoroutine(gamingApi.UploadFile(h => resultHash = h, path, filename));

    Debug.Log(resultHash);
}

private void OnFileUploaded(string hash)
{
    // Apply your desired logic here
    Debug.Log(hash);
}
```

<br/>

### Upload Basic Metadata 

With this method, you will be able to upload metadata with basic attributes to create your basic NFT.  

To implement it, you should create an **IEnumerator** which receives:
- **Name:**   
    - **Type:** String
    - **Description:** The name of the NFT
- **File:**
    - **Type:** String
    - **Description:** The uri of the file that will be the NFT
- **Description:**
    - **Type:** String
    - **Description:** The description of the NFT

Inside this IEnumerator you should create a variable of type **string**. Here is where the **hash** of the upload operation will get stored once the API uploads the metadata.

Then you should start the coroutine **"UploadBasicMetadata"** inside the **"gamingApi"** class, with the **yield return** statement, and with a **delegate** that will set the hash that the API returns as the value of the previously created variable, and all the values recived on the IEnumerator, in the following order:
1. Name
2. File
3. Description

Here is an example of how it should be implemented to upload a metadata and print on the console the **hash** of the upload operation once is completed:

```cs title=apiHandler.cs
private void CallUploadBasicMetadata()
{
    StartCoroutine(UploadBasicMetadata("nftName", "nftFileUri", "nftDescription"));
}

IEnumerator UploadBasicMetadata(string name, string fileUri, string description)
{
    string resultHash = "";

    yield return StartCoroutine(gamingApi.UploadBasicMetadata(h => resultHash = h, name, fileUri, description));

    OnBasicMetadataUploaded(resultHash);
}

private void OnBasicMetadataUploaded(string hash)
{
    // Apply your desired logic here
    Debug.Log(hash);
}
```

<br/>

### Upload Metadata With Numeric Attributes 

With this method, you will be able to upload metadata with numeric attributes to create your NFT with numeric attributes based on the Open Sea Metadata Standard.  

To implement it, you should create an **IEnumerator** which receives:
- **Name:**   
    - **Type:** String
    - **Description:** The name of the NFT
- **File:**
    - **Type:** String
    - **Description:** The uri of the file that will be the NFT
- **Description:**
    - **Type:** String
    - **Description:** The description of the NFT
- **attributes:**
    - **Type:** NumericAttribute[]
    - **Description:** Array of NumericAttribute objects, where each element will be a numeric trait of the NFT

The class "NumericAttribute" is a custom class which represents a numeric trait of a NFT based on the Open Sea Metadata Standard and it have the following properties:
- **Display_type:**
    - **Type:** String
    - **Description:** Type of display on Open Sea
- **Trait_type:**
    - **Type:** String
    - **Description:** The name of the trait of the NFT
- **Value:**
    - **Type:** Float
    - **Description:** The numeric value of the trait of the NFT
- **Max_value:**
    - **Type:** Float
    - **Description:** The maximum value that this attribute can have

You can create a object of this class like this:

```cs
//With max value
NumericAttribute numericAttribute = new NumericAttribute("displayType", "traitType", 5, 10)

//Without max value
NumericAttribute numericAttribute = new NumericAttribute("displayType", "traitType", 5)
```

Inside this IEnumerator you should create a variable of type **string**. Here is where the **hash** of the upload operation will get stored once the API uploads the metadata.

Then you should start the coroutine **"UploadMetadataWithNumericAttributes"** inside the **"gamingApi"** class, with the **yield return** statement, and with a **delegate** that will set the hash that the API returns as the value of the previously created variable, and all the values recived on the IEnumerator, in the following order:
1. Name
2. File
3. Description
4. Attributes

Here is an example of how it should be implemented to upload a metadata and print on the console the **hash** of the upload operation once is completed:

```cs title=apiHandler.cs
private void CallUploadMetadataWithNumericAttributes()
{
    NumericAttribute[] attributes = {
        new NumericAttribute("displayType", "firstTrait", 5, 10),
        new NumericAttribute("displayType", "secondTrait", 7)
    };
    StartCoroutine(UploadMetadataWithNumericAttributes("nftName", "nftFileUri", "nftDescription", attributes));
}

IEnumerator UploadMetadataWithNumericAttributes(string name, string fileUri, string description, NumericAttribute[] attributes)
{
    string resultHash = "";

    yield return StartCoroutine(gamingApi.UploadMetadataWithNumericAttributes(h => resultHash = h, name, fileUri, description, attributes));

    OnMetadataWithNumericAttributesUploaded(resultHash);
}

private void OnMetadataWithNumericAttributesUploaded(string hash)
{
    // Apply your desired logic here
    Debug.Log(hash);
}
```

<br/>

### Upload Metadata With Text Attributes 

With this method, you will be able to upload metadata with text attributes to create your NFT with text attributes based on the Open Sea Metadata Standard.  

To implement it, you should create an **IEnumerator** which receives:
- **Name:**   
    - **Type:** String
    - **Description:** The name of the NFT
- **File:**
    - **Type:** String
    - **Description:** The uri of the file that will be the NFT
- **Description:**
    - **Type:** String
    - **Description:** The description of the NFT
- **attributes:**
    - **Type:** TextAttribute[]
    - **Description:** Array of TextAttribute objects, where each element will be a text trait of the NFT

The class "TextAttribute" is a custom class which represents a text trait of a NFT based on the Open Sea Metadata Standard and it have the following properties:
- **Trait_type:**
    - **Type:** String
    - **Description:** The name of the trait of the NFT
- **Value:**
    - **Type:** String
    - **Description:** The text value of the trait of the NFT

You can create a object of this class like this:

```cs
//With max value
TextAttribute textAttribute = new TextAttribute("traitType", "traitValue")
```

Inside this IEnumerator you should create a variable of type **string**. Here is where the **hash** of the upload operation will get stored once the API uploads the metadata.

Then you should start the coroutine **"UploadMetadataWithTextAttributes"** inside the **"gamingApi"** class, with the **yield return** statement, and with a **delegate** that will set the hash that the API returns as the value of the previously created variable, and all the values recived on the IEnumerator, in the following order:
1. Name
2. File
3. Description
4. Attributes

Here is an example of how it should be implemented to upload a metadata and print on the console the **hash** of the upload operation once is completed:

```cs title=apiHandler.cs
private void CallUploadMetadataWithTextAttributes()
{
    TextAttribute[] attributes = {
        new TextAttribute("firstTrait", "firstTraitValue"),
        new TextAttribute("secondTrait", "secondTraitValue")
    };
    StartCoroutine(UploadMetadataWithTextAttributes("nftName", "nftFileUri", "nftDescription", attributes));
}

IEnumerator UploadMetadataWithTextAttributes(string name, string fileUri, string description, TextAttribute[] attributes)
{
    string resultHash = "";

    yield return StartCoroutine(gamingApi.UploadMetadataWithTextAttributes(r => resultHash = r, name, fileUri, description, attributes));

    OnMetadataWithTextAttributesUploaded(resultHash);
}

private void OnMetadataWithTextAttributesUploaded(string hash)
{
    // Apply your desired logic here
    Debug.Log(hash);
}
```

<br/>

### Prepare Message 

With this method, you will be able to encrypt a message to be able to send it to the player’s wallet for sign the Web3 Login

> :warning: ***This method is already implemented in the wallet connection process and you shouldn’t need to use it***

<br/>

### Validate Message 

With this method, you will be able to check if a message previously prepared with the Prepare Message method has been signed by the player’s wallet, or if it is an impersonation.

> :warning: ***This method is already implemented in the wallet connection process and you shouldn’t need to use it***

<br/>
