# Wallet
## The WalletDB and Object

> The wallet object will look something like this:

```json
{
  "network": "testnet",
  "wid": 1,
  "id": "foo",
  "initialized": true,
  "watchOnly": false,
  "accountDepth": 1,
  "token": "977fbb8d212a1e78c7ce9dfda4ff3d7cc8bcd20c4ccf85d2c9c84bbef6c88b3c",
  "tokenDepth": 0,
  "state": {
    "tx": 0,
    "coin": 0,
    "unconfirmed": 0,
    "confirmed": 0
  },
  "master": {
    "encrypted": false
  },
  "account": {
    "name": "default",
    "initialized": true,
    "witness": false,
    "watchOnly": false,
    "type": "pubkeyhash",
    "m": 1,
    "n": 1,
    "accountIndex": 0,
    "receiveDepth": 1,
    "changeDepth": 1,
    "nestedDepth": 0,
    "lookahead": 10,
    "receiveAddress": "mwfDKs919Br8tNFamk6RhRpfaa6cYQ5fMN",
    "nestedAddress": null,
    "changeAddress": "msG6V75J6XNt5mCqBhjgC4MjDH8ivEEMs9",
    "accountKey": "tpubDDRH1rj7ut9ZjcGakR9VGgXU8zYSZypLtMr7Aq6CZaBVBrCaMEHPzye6ZZbUpS8YmroLfVp2pPmCdaKtRdCuTCK2HXzwqWX3bMRj3viPMZo",
    "keys": []
  }
}
```

Bcoin maintains a wallet database which contains every wallet. Wallets are not usable without also using a wallet database. For testing, the wallet database can be in-memory, but it must be there.

Wallets in bcoin use bip44. They also originally supported bip45 for multisig, but support was removed to reduce code complexity, and also because bip45 doesn't seem to add any benefit in practice.

The wallet database can contain many different wallets, with many different accounts, with many different addresses for each account. Bcoin should theoretically be able to scale to hundreds of thousands of wallets/accounts/addresses.

Each account can be of a different type. You could have a pubkeyhash account, as well as a multisig account, a witness pubkeyhash account, etc.

Note that accounts should not be accessed directly from the public API. They do not have locks which can lead to race conditions during writes.

## Wallet Options
> Wallet options object will look something like this

```json
{
  "id": "walletId",
  "witness": true,
  "watchOnly": false,
  "accountKey": key,
  "accountIndex": 1,
  "type": "pubkeyhash"
  "m": 1,
  "n": 1,
  "keys": [],
  "mnemonic": 'differ trigger sight sun undo fine sheriff mountain prison remove fantasy arm'
}
```
Options are used for wallet creation. None are required.

### Options Object
Name | Type | Default | Description
---------- | ----------- | -------------- | -------------
id | String |  | Wallet ID (used for storage)
master | HDPrivateKey/HDPublicKey | | | Master HD key. If not present, it will be generated
witness | Boolean | `false` | Whether to use witness programs
accountIndex | Number | `0` | The BIP44 [account index](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki#Account)
receiveDepth | Number | | The index of the _next_ receiveing address
changeDepth | Number | | The index of the _next_ change address
type | String | `'pubkeyhash'` |Type of wallet (pubkeyhash, multisig)
compressed | Boolean | `true` | Whether to use compressed public keys
m | Number | `1` | m value for multisig (`m-of-n`)
n | Number | `1` | n value for multisig (`m-of-n`)
mnemonic | String | | A mnemonic phrase to use to instantiate an hd private key. One will be generated if none provided


## Wallet Auth
> The following samples return a wallet object

```javascript
let token, id;
```

```shell--vars
token='977fbb8d212a1e78c7ce9dfda4ff3d7cc8bcd20c4ccf85d2c9c84bbef6c88b3c'
id='primary'
```

```shell--curl
curl $url/wallet/$id/send \
    -H 'Content-Type: application/json' \
    -d '{ "token": "$token" ... }'
```

```shell--cli
bcoin cli wallet get --network=testnet --token=$token
```

```javascript
`use strict`

const httpWallet = new bcoin.http.Wallet({
    id: id,
    token: token,
    network: 'testnet',
});

(async () => {
  const wallet = await httpWallet.getInfo();
  console.log(wallet);
})();
```

Individual wallets have their own api keys, referred to internally as "tokens" (a 32 byte hash - calculated as `HASH256(m/44'->ec-private-key|tokenDepth)`).

A wallet is always created with a corresponding token. When using api endpoints
for a specific wallet, the token must be sent back in the query string or json
body.

e.g. To get information from a wallet that requires a token

`GET /wallet/primary/tx/:hash?token=977fbb8d212a1e78c7ce9dfda4ff3d7cc8bcd20c4ccf85d2c9c84bbef6c88b3c`

## Reset Authentication Token
```javascript
let id, passphrase;
```

```shell--vars
id='foo'
passphrase="bar"
```

```shell--cli
bcoin cli wallet retoken --id=$id --passphrase=$passphrase
```

```shell--curl
curl $url/wallet/$id/retoken \
  -X POST
  --data '{"passphrase":'$passphrase'}"
```

```javascript
const wallet = new bcoin.http.Wallet({ id: id });

(async () => {
  const token = await wallet.retoken(passphrase);
  console.log(token);
})();
```

> Sample response:

```json
{
  "token": "2d04e217877f15ba920d02c24c6c18f4d39df92f3ae851bec37f0ade063244b2"
}
```

Derive a new wallet token, required for access of this particular wallet.

<aside class="warning">
Note: if you happen to lose the returned token, you will not be able to access the wallet.
</aside>


### HTTP Request

`POST /wallet/:id/retoken`

## Get Wallet Info

```shell--curl
curl http://localhost:18332/wallet/test/

```

```shell--cli
bcoin cli wallet get --id=test --network=testnet
```

```javascript
`use strict`

const client = new bcoin.http.Client({  network: 'testnet' });
const id = 'foo';

(async () => {
  const wallet = await client.getWallet(id);
  console.log(wallet);
})();
```

> Output is same as wallet object above

```json
{
  "network": "testnet",
  "wid": 1,
  "id": "foo",
  "initialized": true,
  "watchOnly": false,
  "accountDepth": 1,
  "token": "2d04e217877f15ba920d02c24c6c18f4d39df92f3ae851bec37f0ade063244b2",
  "tokenDepth": 0,
  "state": {
    "tx": 177,
    "coin": 177,
    "unconfirmed": "8150.0",
    "confirmed": "8150.0"
  },
  "master": {
    "encrypted": false
  },
  "account": {
    "name": "default",
    "initialized": true,
    "witness": false,
    "watchOnly": false,
    "type": "pubkeyhash",
    "m": 1,
    "n": 1,
    "accountIndex": 0,
    "receiveDepth": 8,
    "changeDepth": 1,
    "nestedDepth": 0,
    "lookahead": 10,
    "receiveAddress": "mu5Puppq4Es3mibRskMwoGjoZujHCFRwGS",
    "nestedAddress": null,
    "changeAddress": "n3nFYgQR2mrLwC3X66xHNsx4UqhS3rkSnY",
    "accountKey": "tpubDC5u44zLNUVo2gPVdqCbtX644PKccH5VZB3nqUgeCiwKoi6BQZGtr5d6hhougcD6PqjszsbR3xHrQ5k8yTbUt64aSthWuNdGi7zSwfGVuxc",
    "keys": []
  }
}
```

Get wallet info by ID.

### HTTP Request
`GET /wallet/:id`

Parameters | Description
---------- | -----------
id <br> _string_ | named id of the wallet whose info you would like to retrieve

## Get Master HD Key
```javascript
let id, network;
```
```shell--vars
id='foo'
network='testnet'
```

```shell--curl
curl $url/wallet/$id/master
```

```shell--cli
bcoin cli wallet master --id=$id --network=$network
```

```javascript
const wallet = new bcoin.http.Wallet({ id: id,  network: network});

(async () => {
  const master = await wallet.getMaster();
  console.log(master);
})();
```

> Sample response:

```json
{
  "encrypted": false,
  "key": {
    "xprivkey": "tprv8ZgxMBicQKsPe7977psQCjBBjWtLDoJVPiiKog42RCoShJLJATYeSkNTzdwfgpkcqwrPYAmRCeudd6kkVWrs2kH5fnTaxrHhi1TfvgxJsZD",
    "mnemonic": {
      "bits": 128,
      "language": "english",
      "entropy": "a560ac7eb5c2ed412a4ba0790f73449d",
      "phrase": "pistol air cabbage high conduct party powder inject jungle knee spell derive",
      "passphrase": ""
    }
  }
}
```

Get wallet master HD key. This is normally censored in the wallet info route. The provided api key must have admin access.

### HTTP Request

`GET /wallet/:id/master`

Parameters | Description
---------- | -----------
id <br> _string_ | named id of the wallet whose info you would like to retrieve

## Create New Wallet

```javascript
let id, witness;
```

```shell--vars
id = 'foo'
witness = false
```

```shell--curl
curl $url/wallet/$id \
  -X PUT \
  --data '{"witness":'$witness'}'
```

```shell--cli
bcoin cli wallet create $id --witness=$witness
```

```javascript
const client = new bcoin.http.Client();
const options = {
  id: id,
  witness: witness
};

(async() => {
  const newWallet = await client.createWallet(options)
})();
```

> Sample response:

```json
{
  "network": "testnet",
  "wid": 2,
  "id": "foo",
  "initialized": true,
  "watchOnly": false,
  "accountDepth": 1,
  "token": "d9de1ddc83bf058d14520a203df6ade0dc92a684aebfac57b667705b4cac3916",
  "tokenDepth": 0,
  "state": {
    "tx": 0,
    "coin": 0,
    "unconfirmed": "0.0",
    "confirmed": "0.0"
  },
  "master": {
    "encrypted": false
  },
  "account": {
    "name": "default",
    "initialized": true,
    "witness": false,
    "watchOnly": false,
    "type": "pubkeyhash",
    "m": 1,
    "n": 1,
    "accountIndex": 0,
    "receiveDepth": 1,
    "changeDepth": 1,
    "nestedDepth": 0,
    "lookahead": 10,
    "receiveAddress": "muYkrSDbD8UhyWBMXxXf99EKWn22YqmwyF",
    "nestedAddress": null,
    "changeAddress": "mwveV7A6svE5EGGSduZmMKTwcbE775NVFt",
    "accountKey": "tpubDDh2XgSds1vBbeVgye88gsGQeCityoywRndtyrXcmvWqCgsFUyUKwzeDv8HiJhu9fC8jRAFMqxr4jj8eRTNTycmMao5wmsAScVf4jSMdPYZ",
    "keys": []
  }
}
```

Create a new wallet with a specified ID.

### HTTP Request

`PUT /wallet/:id`

Parameters | Description
---------- | -----------
id <br> _string_ | id of wallet you would like to create

<aside class="notice">
See <a href="#wallet-options">Wallet Options</a> for full list and description of possible options that can be passed
</aside>


## Change Passphrase
```javascript
let id, oldPass, newPass;
```

```shell--vars
id='foo'
oldPass = 'oldpass123'
newPass='newpass123'
```

```shell--cli
> No command available
```

```shell-curl
curl $url/wallet/$id/passphrase \
  -X POST
  --data '{"old":'$oldPass', "new":'$newPass'}'
```

```javascript
const client = new bcoin.http.CLient();

(async () => {
  const response = await client.setPassphrase(id, oldPass, newPass);
  console.log(response);
});
```

> Sample Response:

```json
{"success": true}
```

Change wallet passphrase. Encrypt if unencrypted.

### HTTP Request

`POST /wallet/:id/passphrase`

### Body Parameters
Paramters | Description
--------- | ---------------------
old <br> _string_ | Old passphrase. Pass in empty string if none
passphrase <br> _string | New passphrase

<aside class="notice">
  Note that the old passphrase is still required even if none was set prior. In this case, an empty string should be passed for the old passphrase.
  e.g. <code>client.setPassphrase(id,'""', 'newPass')</code>
</aside>


## Unlock Wallet

```javascript
let id, pass, timeout
```

```shell--vars
id='foo'
pass='bar',
timeout=60
```

```shell--cli
bcoin cli wallet unlock --id=$id $pass $timeout
```

```shell--curl
curl $url/wallet/$id/unlock \
  -X POST
  --data '{"passphrase":'$pass', "timeout": '$timeout'}'
```

```javascript
const client = new bcoin.http.Client();
(async () => {
  const response = await client.unlock(id, pass, timeout);
  console.log(response);
})();
```
> Sample Response

```json
{"success": true}
```

Derive the AES key from passphrase and hold it in memory for a specified number of seconds. Note: During this time, account creation and signing of transactions will not require a passphrase.

### HTTP Request

`POST /wallet/:id/unlock`

### Body Parameters
Parameter | Description
--------- | -----------------------
passphrase <br> _string_ | Password used to encrypt the wallet being unlocked
timeout <br> _number_ | time to re-lock the wallet in seconds. (default=60)

## Lock Wallet

```javascript
let id;
```

```shell--vars
id='foo'
```

```shell--cli
bcoin cli wallet lock --id=$id
```

```shell--curl
curl $url/wallet/$id/lock \
  -X POST
```

```javascript
const client = new bcoin.http.Client();
(async () => {
  const response = await client.lock(id);
  console.log(response);
})();
```
> Sample Response

```json
{"success": true}
```

If unlock was called, zero the derived AES key and revert to normal behavior.

### HTTP Request

`POST /wallet/:id/lock`

## Import Public/Private Key

```javascript
let id, account, key;
```

```shell--vars
id='foo'
account='test-account'
key='0215a9110e2a9b293c332c28d69f88081aa2a949fde67e35a13fbe19410994ffd9'
```

```shell--cli
bcoin cli wallet import --id=$id $key
```

```shell--curl
curl $url/wallet/$id/import \
  -X POST \
  --data '{"account":"'$account'", "privateKey":"'$key'"}'
```


```javascript
const wallet = new bcoin.http.Wallet({ id: id });
(async () => {
  const response = await wallet.importPrivate(account, key);
  console.log(response);
})();
```
> Sample Response

```json
{
  "success": true
}
```

Import a standard WIF key.

An import can be either a private key or a public key for watch-only. (Watch Only wallets will throw an error if trying to import a private key)

A rescan will be required to see any transaction history associated with the
key.

<aside class="warning">
Note that imported keys do not exist anywhere in the wallet's HD tree. They can be associated with accounts but will not be properly backed up with only the mnemonic.
</aside>

### HTTP Request

`POST /wallet/:id/import`

### Body Paramters
Paramter | Description
-------- | -------------------------
id <br> _string_ | id of target wallet to import key into
privateKey <br> _string_ | Base58Check encoded private key
publicKey <br> _string_ | Hex encoded public key

> Will return following error if incorrectly formatted:
`Bad key for import`

## Import Address
```javascript
let id, account, address;
```

```shell--vars
id='foo'
account='bar'
address='moTyiK7aExe2v3hFJ9BCsYooTziX15PGuA'
```

```shell--cli
bcoin cli wallet watch --id=$id --account=$account $address
```

```shell--curl
curl $url/wallet/$id/import \
  -X POST \
  --data '{"account":"'$account'", "address":"'$address'"}'
```

```javascript
const wallet = new bcoin.http.Wallet({ id: id });

(async () => {
  const response = await wallet.importAddress(id, account, address)
})();
```

> Sample Response

```json
{
  "success": true
}
```

Import a Base58Check encoded address. Addresses (like public keys) can only be imported into watch-only wallets

The HTTP endpoint is the same as for key imports.

### HTTP Request

`POST /wallet/:id/import`

### Body Paramters
Paramter | Description
-------- | -------------------------
id <br> _string_ | id of target wallet to import key into
address <br> _string_ | Base58Check encoded address

## Get Blocks with Wallet Txs
```javascript
let id;
```

```shell--vars
id="foo"
```

```shell--curl
curl $url/wallet/$id/block
```

```shell--cli
bcoin cli wallet blocks --id=$id
```

```javascript
const httpWallet = new bcoin.http.wallet({ id: id });

(async () => {
  const blocks = await httpWallet.getBlocks();
  console.log(blocks);
}())
```
> Sample Response

```json
[ 1179720, 1179721, 1180146, 1180147, 1180148, 1180149 ]
```

List all block heights which contain any wallet transactions. Returns an array of block heights

### HTTP Request

`GET /wallet/:id/block`

Parameters | Description
-----------| ------------
id <br> _string_ | id of wallet to query blocks with its transactions in it

## Get Wallet Block by Height
```javascript
let id, height;
```

```shell--vars
id="foo"
height=1179720
```

```shell--cli
bcoin cli wallet --id=$id block $height
```

```shell--curl
curl $url/wallet/block/$height
```

```javascript
const httpWallet = new bcoin.http.Wallet({ id: id });

(async () => {
  const blockInfo = await httpWallet.getWalletBlock(height);
  console.log(blockInfo);
})
```

> Sample response:

```json
{
  "hash": "0000000000013cc12ea4b3ff403a3c05d96da695638e468cf26409eca87beb6a",
  "height": 1179720,
  "time": 1503359756,
  "hashes": [
    "2c3d708bc21358e0a8a112ab00267ede0992d85add295d622e2b2c58b2c6720d"
  ]
}
```

Get block info by height.

### HTTP Request

`GET /wallet/:id/block/:height`

Paramaters | Description
-----------| -------------
id <br> _string_ | id of wallet which has tx in the block being queried
height <br> _int_ | height of block being queried


## Add xpubkey (Multisig)
```javascript
let id, key;
```

```shell--vars
id="multi-foo"
key=""
```

```shell--cli
bcoin cli wallet --id=$id shared add $key
```

```shell--curl
curl $url/wallet/$id/shared-key \
  -X PUT
  --data '{"accountKey": $key}'
```

```javascript
const httpWallet = new bcoin.http.wallet({ id: id });
const account = 'default';

(async () => {
  const response = await httpWallet.addSharedKey(account, key);
  console.log(response);
})();
```

> Sample Response

```json
{
  "success": true
}
```

Add a shared xpubkey to wallet. Must be a multisig wallet.

### HTTP Request

`PUT /wallet/:id/shared-key`

### Body Parameters
Paramter | Description
---------| --------------
accountKey <br> _string_ | xpubkey to add to the multisig wallet

## Remove xpubkey (Multisig)

```javascript
let id, key;
```

```shell--vars
id="multi-foo"
key=""
```

```shell--cli
bcoin cli wallet --id=$id shared remove $key
```

```shell--curl
curl $url/wallet/$id/shared-key \
  -X DELETE
  --data '{"accountKey": "'$key'"}'
```

```javascript
const httpWallet = new bcoin.http.wallet({ id: id });
const account = 'default';

(async () => {
  const response = await httpWallet.removeSharedKey(account, key);
  console.log(response);
})();
```

> Sample Response

```json
{
  "success": true
}
```

Remove shared xpubkey from wallet if present.

### HTTP Request

`DEL /wallet/:id/shared-key`

### Body Parameters
Paramter | Description
---------| --------------
accountKey <br> _string_ | xpubkey to add to the multisig wallet


## Get Public Key By Address
```javascript
let id, address;
```

```shell--vars
id="foo"
address="n1EDbjFaKFwz2XwWPueDUac4XZsQg8d1p2"
```

```shell--cli
bcoin cli wallet --id=$id key $address
```

```shell--curl
curl $url/wallet/$id/key/$address
```

```javascript
const httpWallet = bcoin.http.Wallet({ id: id });

(async () => {
  const response = await httpWallet.getKey(address);
  console.log(response);
})();
```

> Sample Response

```json
{
  "network": "testnet",
  "wid": 8,
  "id": "foo",
  "name": "default",
  "account": 0,
  "branch": 0,
  "index": 7,
  "witness": false,
  "nested": false,
  "publicKey": "032b110a0f83d45c1010cf03adea64b440d83a1a3726f7c2d5e94db1d6509b3ac6",
  "script": null,
  "program": null,
  "type": "pubkeyhash",
  "address": "n1EDbjFaKFwz2XwWPueDUac4XZsQg8d1p2"
}
```

Get wallet key by address. Returns wallet information with address and public key

### HTTP Request

`GET /wallet/:id/key/:address`

Parameters | Description
-----------| --------------
id <br> _string_ | id of wallet that holds the address being queried
address <br> _string> | Base58 encoded address to get corresponding public key for

## Get Private Key By Address
```javascript
let id, address;
```

```shell--vars
id="foo"
address="n1EDbjFaKFwz2XwWPueDUac4XZsQg8d1p2"
```

```shell--cli
bcoin cli wallet --id=$id dump $address
```

```shell--curl
curl $url/wallet/$id/wif/$address
```

```javascript
const httpWallet = bcoin.http.Wallet({ id: id });

(async () => {
  const response = await httpWallet.getWIF(address);
  console.log(response);
})();
```

> Sample Response

```json
{
  "privateKey": "cTMUJ7WeFsoQ6dLGR9xdLZeNQafcU88bbibR9TV3W2HheRntYa53"
}

```

Get wallet private key (WIF format) by address. Returns just the private key

### HTTP Request

`GET /wallet/:id/wif/:address`

Parameters | Description
-----------| --------------
id <br> _string_ | id of wallet that holds the address being queried
address <br> _string> | Base58 encoded address to get corresponding public key for


## Generate Receiving Address
```javascript
let id, account;
```

```shell--vars
id="foo"
account="default"
```

```shell--cli
bcoin cli wallet --id=$id address
```

```shell--curl
curl $url/wallet/$id/address -X POST --data '{"account":"'$account'"}'
```

```javascript
const httpWallet = new bcoin.http.Wallet({ id: id });
(async () => {
  const receiveAddress = await httpWallet.createAddress(account);
  console.log(receiveAddress);
})();
```

> Sample response:

```json
{
  "network": "testnet",
  "wid": 1,
  "id": "foo",
  "name": "default",
  "account": 0,
  "branch": 0,
  "index": 9,
  "witness": false,
  "nested": false,
  "publicKey": "02801d9457837ed50e9538ee1806b6598e12a3c259fdc9258bbd32934f22cb1f80",
  "script": null,
  "program": null,
  "type": "pubkeyhash",
  "address": "mwX8J1CDGUqeQcJPnjNBG4s97vhQsJG7Eq"
}
```

Derive new receiving address for account.

<aside class="notice">
Note that, except for the CLI which assumes 'default' account, an account must be passed for the call to work.
</aside>

### HTTP Request

`POST /wallet/:id/address`

### Post Paramters
Parameter | Description
--------- | -------------
account <br>_string_ | BIP44 account to generate address from

## Generate Change Address
```javascript
let id, account;
```

```shell--vars
id="foo"
account="default"
```

```shell--cli
bcoin cli wallet --id=$id change
```

```shell--curl
curl $url/wallet/$id/change -X POST --data '{"account":"'$account'"}'
```

```javascript
const httpWallet = new bcoin.http.Wallet({ id: id });
(async () => {
  const receiveAddress = await httpWallet.createChange(account);
  console.log(receiveAddress);
})();
```

> Sample response:

```json
{
  "network": "testnet",
  "wid": 8,
  "id": "foo",
  "name": "default",
  "account": 0,
  "branch": 1,
  "index": 7,
  "witness": false,
  "nested": false,
  "publicKey": "022f5afafcc8c35dbbbe52842d58dc18d739f2dea85021ea1e9183031032f9fa1c",
  "script": null,
  "program": null,
  "type": "pubkeyhash",
  "address": "mgdArtHtCsxvcjzxRTMfk5ZyBcnsTgNKTT"
}
```
Derive new change address for account.

<aside class="info">
Note that, except for the CLI which assumes 'default' account, an account must be passed for the call to work.
</aside>

### HTTP Request

`POST /wallet/:id/change`

### Post Paramters
Parameter | Description
--------- | -------------
account <br>_string_ | BIP44 account to generate address from

##POST /wallet/:id/nested

Derive new nested p2sh receiving address for account.

### HTTP Request

`POST /wallet/:id/nested`

##GET /wallet/:id/balance

> Sample response:

```json
{
  "wid": 1,
  "id": "foo",
  "account": -1,
  "unconfirmed": "8149.9999546",
  "confirmed": "8150.0"
}
```

Get wallet or account balance.

### HTTP Request

`GET /wallet/:id/balance`

##GET /wallet/:id/coin

List all wallet coins available.

### HTTP Request

`GET /wallet/:id/coin`

##GET /wallet/:id/locked

> Sample response:

```json
[{"hash":"dd1a110edcdcbb3110a1cbe0a545e4b0a7813ffa5e77df691478205191dad66f","index":0}]
```

Get all locked outpoints.

### HTTP Request

`GET /wallet/:id/locked`

##PUT /wallet/:id/locked/:hash/:index

Lock outpoints.

### HTTP Request

`PUT /wallet/:id/locked/:hash/:index`

##DEL /wallet/:id/locked/:hash/:index

Unlock outpoints.

### HTTP Request

`DEL /wallet/:id/locked/:hash/:index`

##GET /wallet/:id/coin/:hash/:index

> Sample response:

```json
[
  {
    "version": 1,
    "height": -1,
    "value": "44.9999546",
    "script": "76a914f4376876aa04f36fc71a2618878986504e40ef9c88ac",
    "address": "n3nFYgQR2mrLwC3X66xHNsx4UqhS3rkSnY",
    "coinbase": false,
    "hash": "0de09025e68b78e13f5543f46a9516fa37fcc06409bf03eda0e85ed34018f822",
    "index": 1
  }
]
```

Get wallet coins.

### HTTP Request

`GET /wallet/:id/coin/:hash/:index`
