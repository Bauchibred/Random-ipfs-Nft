We go through creating an IPFS Hosted NFT 
   1. That uses Randomness to generate a unique NFT

# Getting Started

## Requirements

- [git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
  - You'll know you did it right if you can run `git --version` and you see a response like `git version x.x.x`
- [Nodejs](https://nodejs.org/en/)
  - You'll know you've installed nodejs right if you can run:
    - `node --version` and get an ouput like: `vx.x.x`
- [Yarn](https://yarnpkg.com/getting-started/install) instead of `npm`
  - You'll know you've installed yarn right if you can run:
    - `yarn --version` and get an output like: `x.x.x`
    - You might need to [install it with `npm`](https://classic.yarnpkg.com/lang/en/docs/install/) or `corepack`

## Quickstart

```
git clone https://github.com/PatrickAlphaC/hardhat-nft-fcc
cd hardhat-nft-fcc
yarn
```

## Typescript

If you want to get to typescript and you cloned the javascript version, just run:

```
git checkout typescript
```


# Useage

Deploy:

```
yarn hardhat deploy
```

## Testing

```
yarn hardhat test
```

### Test Coverage

```
yarn hardhat coverage
```



# Deployment to a testnet or mainnet

1. Setup environment variabltes

You'll want to set your `RINKEBY_RPC_URL` and `PRIVATE_KEY` as environment variables. You can add them to a `.env` file, similar to what you see in `.env.example`.

- `PRIVATE_KEY`: The private key of your account (like from [metamask](https://metamask.io/)). **NOTE:** FOR DEVELOPMENT, PLEASE USE A KEY THAT DOESN'T HAVE ANY REAL FUNDS ASSOCIATED WITH IT.
  - You can [learn how to export it here](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key).
- `RINKEBY_RPC_URL`: This is url of the rinkeby testnet node you're working with. You can get setup with one for free from [Alchemy](https://alchemy.com/?a=673c802981)

2. Get testnet ETH

Head over to [faucets.chain.link](https://faucets.chain.link/) and get some tesnet ETH & LINK. You should see the ETH and LINK show up in your metamask. [You can read more on setting up your wallet with LINK.](https://docs.chain.link/docs/deploy-your-first-contract/#install-and-fund-your-metamask-wallet)

3. Setup a Chainlink VRF Subscription ID

Head over to [vrf.chain.link](https://vrf.chain.link/) and setup a new subscription, and get a subscriptionId. You can reuse an old subscription if you already have one. 

[You can follow the instructions](https://docs.chain.link/docs/get-a-random-number/) if you get lost. You should leave this step with:

1. A subscription ID
2. Your subscription should be funded with LINK

3. Deploy

In your `helper-hardhat-config.ts` add your `subscriptionId` under the section of the chainId you're using (aka, if you're deploying to rinkeby, add your `subscriptionId` in the `subscriptionId` field under the `4` section.)

Then run:
```
yarn hardhat deploy --network rinkeby --tags main
```

We only deploy the `main` tags, since we need to add our `RandomIpfsNft` contract as a consumer. 

4. Add your contract address as a Chainlink VRF Consumer

Go back to [vrf.chain.link](https://vrf.chain.link) and under your subscription add `Add consumer` and add your contract address. You should also fund the contract with a minimum of 1 LINK. 

5. Mint NFTs

Then run:

```
yarn hardhat deploy --network rinkeby --tags mint
```


### Estimate gas cost in USD

To get a USD estimation of gas cost, you'll need a `COINMARKETCAP_API_KEY` environment variable. You can get one for free from [CoinMarketCap](https://pro.coinmarketcap.com/signup). 

Then, uncomment the line `coinmarketcap: COINMARKETCAP_API_KEY,` in `hardhat.config.ts` to get the USD estimation. Just note, everytime you run your tests it will use an API call, so it might make sense to have using coinmarketcap disabled until you need it. You can disable it by just commenting the line back out. 



## Verify on etherscan

If you deploy to a testnet or mainnet, you can verify it if you get an [API Key](https://etherscan.io/myapikey) from Etherscan and set it as an environemnt variable named `ETHERSCAN_API_KEY`. You can pop it into your `.env` file as seen in the `.env.example`.

In it's current state, if you have your api key set, it will auto verify kovan contracts!

However, you can manual verify with:

```
yarn hardhat verify --constructor-args arguments.ts DEPLOYED_CONTRACT_ADDRESS
```

### Typescript differences
1. `.js` files are now `.ts`
2. We added a bunch of typescript and typing packages to our `package.json`. They can be installed with:
   1. `yarn add @typechain/ethers-v5 @typechain/hardhat @types/chai @types/node ts-node typechain typescript`
3. The biggest one being [typechain](https://github.com/dethcrypto/TypeChain)
   1. This gives your contracts static typing, meaning you'll always know exactly what functions a contract can call. 
   2. This gives us `factories` that are specific to the contracts they are factories of. See the tests folder for a version of how this is implemented. 
4. We use `imports` instead of `require`. Confusing to you? [Watch this video](https://www.youtube.com/watch?v=mK54Cn4ceac)
5. Add `tsconfig.json`

# Linting

To check linting / code formatting:
```
yarn lint
```
or, to fix: 
```
yarn lint:fix
```

# Thank you!

If you appreciated this, feel free to follow me or donate!

ETH/Polygon/Avalanche/etc Address: 0x9680201d9c93d65a3603d2088d125e955c73BD65

[![Patrick Collins Twitter](https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white)](https://twitter.com/PatrickAlphaC)
[![Patrick Collins YouTube](https://img.shields.io/badge/YouTube-FF0000?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/channel/UCn-3f8tw_E1jZvhuHatROwA)
[![Patrick Collins Linkedin](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/patrickalphac/)
[![Patrick Collins Medium](https://img.shields.io/badge/Medium-000000?style=for-the-badge&logo=medium&logoColor=white)](https://medium.com/@patrick.collins_58673/)


Here we are creating a Random Ipfs Nft
 
 so here what we want is when we mint a NFT, we will trigger a chainlink VRF call to get us a random number, using that number we will get a random Nft out of the three that's available for us, which is the Pug, a Shiba Inu or a St. Bernard
 And we are going to make all our Nfts to have different type of rarity
 The pug being the most rare
 Shiba sort of rare ad then th St. Bernard being common
Now we have to also pay so as to be able to access the mint function
And we as the owner have to have access to be able to withdraw the ETH from the contract
 Then asides that
 so we need to make a requestNft function and a fulfillRandomWords function
and out token uri

Since we need a chainlink vrf to request for our random number we add it by running the below command
yarn add --dev @chainlink/contract
then we import the VRFConsumerBaseV2 and our interface then we inherit the VRFConsumerBaseV2 as our RandomIpfsContrac using the "is" word

we have to create a constructor for us to be able to access our requestNft function

under our constuructor we have to place 
   i_vrfCoordinator = VRFCoordinatorV2Interface(vrfCoordinatorV2);
   and also make sure that we have i_vrfCoordinator set as a global variable
   from the chainlink documentation we can see other variables we are going to need so we add it for e.g our subscriptio Id, keyhash and the rest so we add that to our constructor after stating them as global variables under the contract
       VRFCoordinatorV2Interface private immutable i_vrfCoordinator;
    uint64 private immutable i_subscriptionId;
    bytes32 private immutable i_gasLane;
    uint32 private immutable i_callbackGasLimit;
    uint16 private constant REQUEST_CONFIRMATIONS = 3;
    uint32 private constant NUM_WORDS = 1;
    After doing this we add these to our constructor
After having all these variables we can now work on the requestNft function
and request a random number to get our randomNft
Here requesting our random nft goes in 2 transactions and if one where the owner requests the random number from chainlink and the second one is when chainlink calls the function to request the Nft, normally we are to use msg.sender as the owner, but if we use that under our fullfillrandomwords function, chainlink becomes the owner of the Nft which we din't want so we are going to need to create a mapping of the requestId with the person that called that particular requestNft function
And we create it under our VRF Helpers, and they should be private
mapping(uint256 => address) public s_requestIdToSender;
so when we now call the requestnft function we add
      s_requestIdToSender[requestId] = msg.sender;

Then we add
  address dogOwner = s_requestIdToSender[requestId];
to our fulfillrandomwords function that way when the chainlink nodes respond, they are not the dogOwner but rather the person who called the requestNft function
now we have the user so now we have to create the token counter we use s_ since this is a tojen counter and add it as an Nft variable
but for us to safemint we have to import the ERC721 from openzeppelin

Creating Rare Nfts
for this we have to create aa function i.e our getChanceArray function which is a public pure, by making this array 

   return [10, 30, MAX_CHANCE_VALUE]; 
   so this means that index 0 has a 10% probability of us getting it, index 1 is 20% cause the maths is 30 - 10 while index 2 has a 60% chance
   so we add the moddedRng to our fullfill reanowm words function and then create a getbreedfromModdedRng that's goign to tale pur moddedRng and it's going to return the breed of the dog
   then we create an enum for our breed
      enum Breed {
        PUG,
        SHIBA_INU,
        ST_BERNARD
    }
    i.e the 0th position is a bug, the first is shiba inu and the last one is st_bernard
    we use the below if statement to get what particular breed our dog is going to be
      if (moddedRng >= cumulativeSum && moddedRng < chanceArray[i]) {
                return Breed(i);
            }
then if it's not we now set our cummulative breed to   cumulativeSum = chanceArray[i]; which would change since we've ran through the first loop we had

then in case it doesn't work we add an error and also globalise it in the beginning of the econtract
so since we can now get the breed, we now add it to our fuflfill function, then we safemint
under the openzeppelin ERC721, we have to set the tokenURI ourselves, though this function is not the most gas efficient, it is the one with the most customization so we would be using it for this project

So now we need to import ERC721 storage and inherit for our contract
but now our constructor still uses the ERC721 and we done need to change that cause this ERC721storage is just an extension that comes with othe useful functions like the setTokenUri
now we add a string Array of size 3 which is going to be a list of the tokenUris for all our 3 different dog breeds
then under our fulfill randomwords function we pass in the 
     _setTokenURI(newItemId, s_dogTokenUris[uint256(dogBreed)]);
here we are back changing the dogbreed to a uint 256 so our s_dogTokenUris can then get the token uri for the specific breed of the dog using the index

Setting an NFT mint Price
so to do this we just make our requestNft a public payable function and then add an if condition 
  if (msg.value < i_mintFee) {
            revert NeedMoreETHSent();
        }
then under our constructor we add our mint fee, and then ad an error and globalise the error
for withdrawal we would like only the owner to be able to withdraw from the contract
openzeppelin already has an ownable acesss under their contracts where we can use th eonly owner modifier so we can just import that to file and then inherit on our contract using the "is" and then we make our withdraw function only owner, so now whoever deployed the contract which is us in this case is going to be the only one allowed to withdraw from this function
then under the function we add the 
  uint256 amount = address(this).balance;
        (bool success, ) = payable(msg.sender).call{value: amount}("");
        require(success, "Transfer failed");
        or we can change the require success to an if statement and add the error to if it does not go through.
then below we add all the functions we are going to need and what they return
we add our events cause when we requestNft we emit an event that we pass in requestedId and our msg.sender
so now we globalise our events
Then when we fulfill we emit the NftMinted event which is going to take the dogbreed ad the dog owner

The Deploy script
This is the interesting part of the codes

so since we are working with chainlink we are going to need to have our mocks

we state our vrfcoordinatorV2Address, subscription Id by using 
 let vrfCoordinatorV2Address, subscriptionId
  then we chcck our helper hardhat to make sure that all the params we need are added under our network config
  we add our arguments for the gas lane, mintfee, callback gas limit and our token uris 

  Uploading Images with Pinata

  so now we need to get the ipfs hasthes for our images from our images folder, but if we host it on our server and we are the only node that's running this, this means that it's centralised which is not what we want, 
  Which is why we use Pinata, the only issue with pinata is that we are like paying just one central entitty to pin our data, giving them full trust that they are actually going to pin it and they are not going to go down, another way is using Nft storage, but this uses filecoin at the back, which is one of the persistent way we have for uploading our data

  so now to upload to pinata we first use an if statement to knwo if we want to upload to pnata or not and then create our handleTokenUris() async function which returns an array of token uri for us to upload to our smart contracts
  to do this so we need to store our images on ipfs and then store the metadata also on ipfs
  So now under our utils we create a new file to upload to pinata

  so now we upload on pinata using our api key to access our pinata profile
  so we run 
  yarn add --dev @pinata/sdk 
  as the sdk is what we would use to pin our hashes on pinata
  so the inpiont we are going to be using is pinFileToIPFS and also pinJSONToIPFS as the metadata we are going to be pinning is going to be a json
  we are going to run 
  yarn add --dev path 
  to help us with our paths
    const fullImagesPath = path.resolve(imagesFilePath) 
This just provides us with the full images from the path and we dont need no ./images/random
    const files = fs.readdirSync(fullImagesPath)
this is to ensure that we are reading the files correctly

for each of the files in our file stream we are going to make the datat readable cause right now they are just images but they are also data which contain you know bytes and all that, so to do this we use the line
        const readableStreamForFile = fs.createReadStream(`${fullImagesPath}/${files[fileIndex]}`)
so to now run this we use try catches
and under the try we pinJSONToIPFS, but before that we have to create our pinata api key and secret, then we globalise them on our uploadtopinata file
