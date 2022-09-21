# Plane Garage TON

A library to deploy and manage editable nft collections, NFT, deploy-wallet and proxy-contact (minter of collections) for Plane Garage on TON.

As mentioned above, basic architecture consists of 4 smart-contracts:

1. **Deploy-wallet** will be the owner of all other smart-contracts and will be used to send internal messages to other smart-contracts (e.g. to send message to collection for it to deploy new NFT or just load other smart-contract with coins)
2. **Collection-minter** is a special smart-contract that stores the code of collection smart-contact and allows us to deploy new collections just sending an extrernal message to this contract with collection content using lite-cliens (of course this part is abstracted in this library making it even simplier)
3. **Collection** is a smart-contract of the collection, that has collection content in it's storage (link to JSON-file of collection) and some other data
4. **NFT** is a smart-contract of particular NFT, that has NFT content in it's storage (link to JSON-file) and some other data

This library has the following features:

- [Compile deploy-wallet](#compile-deploy-wallet)
- [Mint deploy-wallet](#mint-deploy-wallet)
- [Mint collection](#deploy_collection)
- [Add collection address to file](#add-collection-addr-to-file)
- [Mint NFT](#deploy_nft)
- [Edit collection](#edit_collection)
- [Edit NFT](#edit_nft)
- Transfer NFT (Coming soon)

## Installation

### Step 1: Check ton-binaries

By default this library was made to run on Ubuntu 18.04 and this library contains ton-binaries for it, but if you want to use this library on other OS download nessesary TON pre-build [here](https://github.com/ton-blockchain/ton/actions?query=branch%3Amaster+is%3Acompleted) and change directories to lite-client, fift and func compilers on lines 6-8 of [shell_config.sh](config/shell_config.sh) as follows:

```bash
path_to_func_binaries=YOUR_PATH # func
path_to_fift_binaries=YOUR_PATH # fift
path_to_lite_client_binaries=YOUR_PATH # lite-client
```

<a name="compile-deploy-wallet"/></a>

### Step 2: Compile deploy-wallet

First you need to compile your deploy wallet, that will send internal messages to other smart-contracts in the architecture and will be the owner of the contracts. After compiling the wallet you will find files with private key, wallet address and resulting BOC-file that we will send to blockchain on deploy in the directory [src/build/wallet/](src/build/wallet/)

Example:

```bash
sudo sh use.sh compile-wallet
```

### Step 3: Load TON's

Load coins to the address of your future deploy-wallet using [Testnet TON Wallet](https://wallet.ton.org/?testnet=true) in testnet and [TON Wallet](https://wallet.ton.org), [Tonkeeper](https://tonkeeper.com) or [Tonhub](https://tonhub.com) in mainnet.

You will find the address of your future wallet (where you need to send coins) in the output of the terminal after making the previous step. Search a line in output like this:

> new wallet address = COPY_ADDRESS_THAT_WILL_BE_HERE

<a name="mint-deploy-wallet"/></a>

### Step 4: Mint deploy-wallet

Use command below to deploy wallet:

> sudo sh use.sh deploy-wallet [net]

| Argument | Description |
| --- | --- |
| [net] | Stipulate "testnet" or "mainnet" |

Example:

```bash
sudo sh use.sh deploy-wallet testnet
```

<a name="deploy_collection"/></a>

## Mint collection

Use pattern below to deploy your collection.

> sudo sh use.sh deploy-collection [net] [gateway_base_uri] [collection_uri] [royalty percent * 10]

| Argument | Description |
| --- | --- |
| [net] | Stipulate "testnet" or "mainnet" |
| [gateway_base_uri] | Base URI of the Web3 gateway you use (everything in URL exept CID of the particular file on IPFS), e.g. https://cloudflare-ipfs.com/ipfs/ or https://ipfs.io/ipfs/. You can find full list of available gateways for IPFS [here](https://ipfs.github.io/public-gateway-checker/) |
| [collection_uri] | Full URL to the JSON-file of your collection |
| [royalty percent * 10] | Integer that stands for royalty, e.g. if you want to set royalty to 2,5% you should stipulate "25" as an argument |

Example:

```bash
sudo sh use.sh deploy-collection testnet https://cloudflare-ipfs.com/ipfs/ https://cloudflare-ipfs.com/ipfs/bafkreifdmjqhqvh6tpmqnhss3jb3qd7oeg6lbafqi5pyuyxh7ihg3nxw6m 50
```

<a name="deploy_collection"/></a>

## Add collection address to file

<a name="add-collection-addr-to-file"/></a>

This method creates file src/build/last_collection.txt and writes the address of the lastly deployed collection in it. You can also pass an argument with your file and the method will append the collection address to it. Use pattern below to add collection address to file.

> sudo sh use.sh add-collection-addr-to-file [user_file_directory]

| Argument | Description |
| --- | --- |
| [user_file_directory] | Optional. Stipulate the directory of the file you want to append the lastly deployed collection to |

Example:

```bash
sudo sh use.sh add-collection-addr-to-file
```

## Mint NFT

Use the following pattern to deploy NFT

> sudo sh use.sh deploy-nft [net] [collection_addr] [nft_index] [CID]

| Argument | Description |
| --- | --- |
| [net] | Stipulate "testnet" or "mainnet" |
| [collection_addr] | Address of the smart-contract of your collection in user-friendly format |
| [nft_index] | Sequence number of the NFT you are deploying in the collection (do not confuse with SEQNO in wallet smart-contracts) |
| [CID] | CID of the JSON-file of your particular NFT that you get on IPFS |

Example:

```bash
sudo sh use.sh deploy-nft testnet EQCNzB_nhErFna-OgXaeszHF_CvkdZPCuCow5pngjFErJvfe 0 bafkreic27uqjmsam4ghn7gch4ldhok7h2wthzdtc7stpjdrvlitwxh3mqi
```

<a name="edit_collection"/></a>

## Edit collection

Use pattern below to edit your collection.

> sudo sh use.sh edit-collection [net] [collection_addr] [new_base_uri] [new_collection_uri] [new_royalty_numerator]

| Argument | Description |
| --- | --- |
| [net] | Stipulate "testnet" or "mainnet" |
| [collection_addr] | Address of the smart-contract of your collection in user-friendly format |
| [new_base_uri] | Base URI of the Web3 gateway you use (everything in URL exept CID of the particular file on IPFS), e.g. https://cloudflare-ipfs.com/ipfs/ or https://ipfs.io/ipfs/. You can find full list of available gateways for IPFS [here](https://ipfs.github.io/public-gateway-checker/) |
| [new_collection_uri] | Full URL to the JSON-file of your collection |
| [new_royalty_numerator] | Integer that stands for royalty, e.g. if you want to set royalty to 2,5% you should stipulate "25" as an argument |

Example:

```bash
sudo sh use.sh edit-collection testnet EQDkQFpi0wJL08wrPErslfr3i2_pIx4JyLVrfvDzMsavDhYA https://ipfs.io/ipfs/ https://ipfs.io/ipfs/bafkreicqouhcwhkiza7ogvpo7ncmzzhhfdvdt65xjdhmycjegqwyh6e3h4 10
```

You can change data back after testing:

```bash
sudo sh use.sh edit-collection testnet EQDkQFpi0wJL08wrPErslfr3i2_pIx4JyLVrfvDzMsavDhYA https://cloudflare-ipfs.com/ipfs/ https://cloudflare-ipfs.com/ipfs/bafkreifdmjqhqvh6tpmqnhss3jb3qd7oeg6lbafqi5pyuyxh7ihg3nxw6m 50
```

<a name="edit_nft"/></a>

## Edit NFT

Use pattern below to edit your NFT.

> sudo sh use.sh edit-nft [net] [nft_addr] [new_cid]

| Argument | Description |
| --- | --- |
| [net] | Stipulate "testnet" or "mainnet" |
| [collection_addr] | Address of the smart-contract of your NFT in user-friendly format |
| [new_cid] | CID of the new JSON-file of your particular NFT that you get on IPFS |

Example:

```bash
sudo sh use.sh edit-nft testnet EQD1PZVkaUM1jAaf12aChzlLImJZj3IPjS2BfSzunuPY4Lm2 bafkreifhuhy3u3c2lvu26us6kulx64gicfjy7uvu5vnrf5c6qsejxaeaza
```

You can change data back after testing:

```bash
sudo sh use.sh edit-nft testnet EQD1PZVkaUM1jAaf12aChzlLImJZj3IPjS2BfSzunuPY4Lm2 bafkreic27uqjmsam4ghn7gch4ldhok7h2wthzdtc7stpjdrvlitwxh3mqi
```