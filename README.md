# DirtyDog Solana NFT 

![image](https://github.com/sairammohandass/DirtyDog-NFT-Solana/assets/98868282/19e83f1f-3a60-4863-820a-b2766228ad76)

Hey there 👋🏼


# Accounts That I Used:
1. [Github Account](https://github.com/)
2. [Vercel Account](https://vercel.com/)
3. [Alchemy API Key](https://alchemy.com/?a=solana-nft-tutorial) 
5. [Node](https://nodejs.org/en/)(v16.15.1) and [Yarn](https://yarnpkg.com/)

# What did I build..
Solana NFT Collection. Generated the images and metadata with the art generation tool, HashLips. Followed with the Sugar CLI tool to deploy the Candy Machine program to facilitate my NFT collection on Solana's devnet. At last, deployed the NFT minting application to production with Vercel and manually tested its functionality.

Here's what the frontend DApp will look like:
(![image](https://github.com/sairammohandass/DirtyDog-NFT-Solana/assets/98868282/5acaee05-5bea-4d4b-afff-ef625c6ec586)

When you're done, you'll be able to mint our NFTs using any Solana wallet (i.e. Phantom) on the devnet via Alchemy's Solana RPC.
(![image](https://github.com/sairammohandass/DirtyDog-NFT-Solana/assets/98868282/f14dbb4b-5b90-439e-a193-1fd966bcc74e)

<!-- 1. Art and Metadata generation with HashLips -->
<!-- 2. NFT Collection Deployment with Metaplex Sugar CLI and Candy Machine -->
<!-- 3. Minting Dapp Deployment with React and Vercel -->
<!-- 4. Testing -->

#CLI Format

```sh
# shell
echo "this is a cli command"
```

```sh
# output
this is a cli command
```

# Where we'll work

Using the following commands below, change into our home directory and create a new directory. This will serve as our workspace to isolate our files during this tutorial so change our current directory to our newly created folder.



# Solana Tool Suite

### Installation

installing Solana's suite of tools by following [this guide](https://docs.solana.com/cli/install-solana-cli-tools).

To check if installed properly by running the command below:

```sh
# shell
solana --version
```

```sh
# output
solana-cli 1.10.24 (src:15456493; feat:2982808786)
```
#Setting configuration.



`solana config set` - set the current Solana tool suite configuration.

`solana-keygen new` - create a new filesystem keypair.

`solana airdrop <amount> <address>` - airdrop some devnet / testnet Solana in a certain address.


 check the current configuration of Solana tool suite with `solana config get`.

```sh
# shell
solana config get
```

You should receive an output similar to the one below. The key things to look out for are the `RPC URL` and our `Keypair Path`. These 2 are the ones you will most likely change in order to do some testing.
```sh
# output
Config File: /home/sairam/.config/solana/cli/config.yml
RPC URL: https://api.mainnet-beta.solana.com
WebSocket URL: wss://api.mainnet-beta.solana.com/ (computed)  //changing to devnet
Keypair Path: /home/sairam/.config/solana/id.json
Commitment: Confirmed
```

 create a Solana-devnet application and generate an HTTPS URL: `https://solana-devnet.g.alchemy.com/v2/<our-api-key>`. 

```sh
# shell
solana config set --url https://solana-devnet.g.alchemy.com/v2/<our-api-key>
```

You should get a similar output below.
```sh
# output
Config File: /home/sairam/.config/solana/cli/config.yml
RPC URL: https://solana-devnet.g.alchemy.com/v2/<our-api-key>
WebSocket URL: wss://solana-devnet.g.alchemy.com/v2/<our-api-key> (computed)
Keypair Path: /home/sairam/.config/solana/id.json
Commitment: confirmed
```
# Generating Assets with HashLips

### Setup
After installing Solana's tool suite, it's time to generate some  assets with **HashLips**.

HashLips is an art generation tool that can be used to layer images on top of each other depending on our own configuration. It's a tool that's mainly used for generative art collections in Solana.


```sh
# shell
# current directory: /home/sairam/sairam-nft-solana
git clone https://github.com/alchemyplatform/solana-nft-tutorial // cloning from alchemy repo.
```

You should receive directory structure similar to below with the pre-made layer images we will be using for the art generation we will be doing with HashLips.
```
/sairam-nft-solana
	README.md
	/template
		/0-assets
			/Head
				HotPink#1.png
				Pink#1.png
				Yellow#1.png
			/Mouth
				Frown#1.png
				Smile#1.png
				Shock#1.png
			/Eyes
				Circular#1.png
				Oval#1.png
				Wonky#1.png
		/1-generate
		/2-build
		/3-deploy
```



Go to the root of the newly cloned repository and install the dependencies.
```sh
# shell
# current directory: /home/sairam/sairam-nft-solana/template/1-generate/hashlips_art_engine
yarn install # yarn
npm install # npm
```

### Usage

HashLips is a very powerful art engine that allows you to create different configurations for our art generation, but for this tutorial's purposes **we will keep it as simple as possible**.



Copy all of the folders inside `0-assets` in the template into `layers` folder because that's where all of our layers will be placed for art generation using **HashLips**.

After copy and pasting the folders from `0-assets` our `layers` folder should look like the one below.
```

/layers
	/Head
		dirtydog2.png
		dirtydog3.png
		dirtydog4.png
	/Mouth
		mouth1.png
		mouth2.png
		
	/Eyes
		eyes1.png
		eyes2.png
		eyes3.png
```


First things first, modify the `network` from `NETWORK.eth` to `NETWORK.sol`.
```js
const network = NETWORK.sol
```

Change of `namePrefix` to the name of my collection. In this case we're calling it **DirtyDog**. Also change `description` to the description of our NFT collection.
```js
const namePrefix = "DirtyDog";
const description = "xyz";

```

Reference Metaplex Docs(https://docs.metaplex.com/programs/token-metadata/token-standard).

This is the default Solana metadata in `config.js`.
```js
const solanaMetadata = {
  symbol: "YC",
  seller_fee_basis_points: 1000, // Define how much % you want from secondary market sales 1000 = 10%
  external_url: "sairammohandass.com",
  creators: [
    {
      address: "devnet address",
      share: 100,      // 1 %
    },
  ],
};
```



```

`layerConfigurations`
The configuration below states the ordering of the asset generation.

The top layer added `{ name: "Head" }` is the first asset to be drawn hence the furthest from the front.

The bottom layer `{ name: "Eyes" }` is the last asset to be drawn hence the nearest to the front.


```js

const layerConfigurations = [
  {
    growEditionSizeTo: 10,
    layersOrder: [
      { name: "Head" },
      { name: "Mouth" },
      { name: "Eyes" },
    ],
  },
];
```

Now that done with the setup, it's time to build. 
```sh
# shell
yarn run build # yarn
npm run build # npm




```
[Output after generation](https://github.com/sairammohandass/DirtyDog-NFT-Solana/assets/98868282/0c7d7a64-03ca-4c74-9a4f-492401a56ccc)



```sh
# current directory: /home/sairam/sairam-nft-solana/template/1-generate/hashlips_art_engine
/build
	/images
		0.png
		1.png
		.
		.
		
	/json
		_metadata.json
		0.json
		1.json
		.
		.
		
```

![image](https://github.com/sairammohandass/DirtyDog-NFT-Solana/assets/98868282/492f0ea7-55bf-4b83-a2df-d7413c8f2fbd)

# Deploying our Collection

Metaplex built a has a tool called [Sugar](https://docs.metaplex.com/developer-tools/sugar/) that's a CLI tool used for managing our deployed `Candy Machine Program`. The `Candy Machine` is used for minting and distribution of Solana NFTs.

Using `Sugar` will make our deployment and management easier.

### Installation

```sh
# shell
bash <(curl -sSf https://sugar.metaplex.com/install.sh)
```

Confirm that it was installed correctly.

```sh
# shell
sugar --version
```

```sh
# output
sugar-cli 1.1.0
```

### `sugar` Usage

##### Preparing the Assets
Change directory into `2-build` in the template. This will serve as our workspace for using `sugar` and deploying the collection.

Next create an `assets` directory in the `2-build` folder and copy the images and JSON file from HashLips generation earlier.

After copying the files from our `2-build` directory should look similar to the one below.

```
/sairam-nft-solana
	** snipped **
	/2-build
		/assets
			_metadata.json
			0.png 0.json
			1.png 1.json
			2.png 2.json
			.
			.
			.

	/3-deploy
```

Notice the `_metadata.json` file? Move it to a different folder to make sure only the images and JSON files are in the asset folder.

##### Collection Mint

Collection mint is the NFT that serves as the "album cover" of our NFT collection in the marketplace. 

To set the collection mint automatically go into the `assets` folder and copy `0.png` into `collection.png` and `0.json` into `collection.json`.

After removing `_metadata.json` and adding a collection mint our assets directory should be similar to the one below.

```
/solana-nft-tutorial
	** snipped **
	/2-build
		/assets
			collection.png
			collection.json
			0.png 0.json
			1.png 1.json
			2.png 2.json
			.
			.
			.
			25.png 25.json
			26.png 26.json
	/3-deploy
```

Now that our assets are ready and combined in our `assets` folder as seen below, it's time to use `sugar` in order to deploy it.


![Assets Folder Overview]

### Deployment

Before we start make sure you change directory into the `2-build` folder where the `assets` folder is found.

##### Overview

1. `sugar validate` to check if the images and metadata was generated correctly
2. `sugar create-config` to create a **Candy Machine configuration** interactively
2. `sugar upload` to upload our images and metadata (JSON) files to the storage of our choice
3. `sugar launch` to launch our Candy Machine for NFT collection minting
4. `sugar verify` to verify uploaded data
5. `sugar show` to show the details of our launched Candy Machine

##### Step-by-Step

In the `2-build` folder where the `assets` folder is found, run the following commands below.

Validate if the generated images and metadata (JSON) are valid and ready to be deployed.

```sh
# shell
sugar validate
```

You must receive the same output below with a command successful event if our images and metadata are ready for upload.

```sh
# output
[1/1] 🗂  Loading assets
▪▪▪▪▪ Validating 28 metadata file(s)...

Validation complete, our metadata file(s) look good.

✅ Command successful.
```

After validating our generated images and metadata, it's time to interactively create a **Candy Machine** config using `sugar create-config`.

First make sure you have a local filesystem wallet generated by `solana-keygen` by running the command below.

```sh
# shell
solana-keygen new
```

When prompted for a passphrase just press enter for now to skip the passphrase. If everything is done correctly you should now have a keypair JSON file in a similar path: `/home/kristian/.config/solana/id.json`.

> Warning: The authority for our Candy Machine is the keypair specified in the output of the `solana config` command. For this tutorial it should be the path of our previously generated keypair with `solana-keygen new` that is `/home/kristian/.config/solana/id.json`.

After generating our keypair and making sure it's the one used by our `solana config`, run the command below to begin the interactive config creation.

```sh
# shell
sugar create-config
```

> Note: Feel free to play around with the interactive config creation. Metaplex does a good job of handling errors, so rest assured that you will be guided by error messages along the way.

You will then be prompted for the following:

1. `Price of each NFT` - This is the mint price fo each NFT in our collection
2. `Number of NFTs in the collection` - This is the number of NFTs in our collection. **For this tutorial it's `27` in total**
3. `Symbol` to be used in the collection - Just like `BAYC` for Bored Ape Yacht Club. We use `PS` for this tutorial.
4. `Seller fee basis points` (1000 = 10%) - the amount of royalty to be earned from secondary market sales
5. `Go live date` - date from when the collection becomes available for minting. You can use the keyword `now` to signify that it's ready for minting upon launch.
6. `Number of creator wallets` - how many wallets would receive the royalties from secondary sales
7. `Wallet address for each creator` - addresses of the wallets that will receive the royalties
8. `Royalty percentage for each creator` - the percentage of the total royalty for each sale each creator should get. All of the percentages from each wallet combined should total to `100`
9. `Extra Features` - additional features to be toggled for our Candy Machine. This will be discussed further in the section below

In order to keep this simple for this tutorial we won't specify the other prompts and we recommend you follow the same responses below (colored in green).

![create-config Sample Configuration](https://raw.githubusercontent.com/alchemyplatform/solana-nft-tutorial/master/.github/images/create-config.png)

##### Extra Features

The interactive prompt will prompt you with some extra features that you can use to cover different use cases. Covering each is beyond the coverage of this tutorial but you can know more by [reading the configuration settings from Metaplex](https://docs.metaplex.com/developer-tools/sugar/reference/configuration).

##### Uploading to Storage

After going through the interactive config creation, it's time to upload our collection to storage.

```sh
# shell
sugar upload
```

If the upload is successful, you should get a similar output below.

```sh
# output
[00:00:00] Upload successful ██████████████████████████████████████████████████ 28/28

11/11 asset pair(s) uploaded.

✅ Command successful.
```

Upon the successful completion of the upload it's time to launch our Candy Machine!

```sh
# shell
sugar launch
```

If you got a **Command Succesful** response, you're almost done!

Do a quick `sugar verify` followed by a `sugar show` to check the status of our launched Candy Machine and check for a **Command Successful** response.

```sh
# shell
sugar verify
sugar show # check details of deployed Candy Machine
```

Take note of the **Candy Machine ID**. You'll use it for deploying the frontend.

**Congratulations! You're done!** with deploying our Candy Machine! It's time to use Metaplax's Candy Machine template to mint our NFTs!

# Deploy Minting Website



 Metaplex's [Candy Machine UI](https://github.com/metaplex-foundation/candy-machine-ui) repository and forked it.

![Candy Machine UI Repo](https://raw.githubusercontent.com/alchemyplatform/solana-nft-tutorial/master/.github/images/candy-machine-ui.png)

After forking it go to [Vercel](https://vercel.com/) login and add new **Project** and **Continue with Github**. Choose our fork of the **Candy Machine UI**. 

![Vercel Add New](https://raw.githubusercontent.com/alchemyplatform/solana-nft-tutorial/master/.github/images/add-new.png)

##### Configure Environment Variables

Check our fork and open `.env.example` use it to configure our Vercel Environment Variables.

You can copy the contents below.

```sh
REACT_APP_CANDY_MACHINE_ID=<CANDY MACHINE PROGRAM ID>

REACT_APP_SOLANA_NETWORK=devnet
REACT_APP_SOLANA_RPC_HOST=https://solana-devnet.g.alchemy.com/v2/<api-key>
SKIP_PREFLIGHT_CHECK=true
```


After configuring it's time to deploy and **view our site**!

![Deploying](https://raw.githubusercontent.com/alchemyplatform/solana-nft-tutorial/master/.github/images/deploy.png)

A webpage should open like the one below. Connect our wallet and try minting one to test our Candy Machine deployment.

![Connect Wallet](https://raw.githubusercontent.com/alchemyplatform/solana-nft-tutorial/master/.github/images/connect-wallet.png)

# Testing

First get our devnet Solana (SOL) using Solana CLI and with our Phantom wallet address.


```sh
# shell
solana airdrop 2 **insert wallet address here**
```

After getting our Devnet SOL, it's time to finally try our Minting Dapp by connecting our wallet and minting our first Devnet NFT!

![image](https://github.com/sairammohandass/DirtyDog-NFT-Solana/assets/98868282/c537a843-1f92-4570-a37b-a91f418fab5c)

### What else moving forward...

NFTs can have a lot of utilities and a simple collection that you've just deployed is just the start. If you want to know more and find more ways to use Solana NFTs as well as to add utilities to them learn from the following resources below:

- [Generative Art Tips and Tricks](https://www.youtube.com/watch?v=HGx052UU8A0&t=19s)
- [Top 10 Solana NFT Collections: List of the Most Ambitious Projects, Ranking, Trading Volume](https://mpost.io/top-10-solana-nft-collections-list-of-the-most-ambitious-projects-ranking-trading-volume/)
- [DeGods: The Recent Solana NFT Project Ascending in Rankings](https://learn.bybit.com/nft/what-are-degods-nfts/)


##### 
