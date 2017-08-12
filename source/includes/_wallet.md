# Wallet API
## The WalletDB and Object

> The wallet object will look something like this:

```json
{
	"network": "testnet",
	"wid": 1,
	"id": "primary",
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



## Wallet Auth
> The following samples return a wallet object

```shell--vars
    token='977fbb8d212a1e78c7ce9dfda4ff3d7cc8bcd20c4ccf85d2c9c84bbef6c88b3c'
    id='primary'
```

```shell--curl
curl http://localhost:18332/wallet/$id/send \
    -H 'Content-Type: application/json' \
    -d '{ "token": '$token' ... }'
```

```shell--cli
bcoin cli wallet get --network=testnet --token=$token
```

```javascript
`use strict`

const httpWallet = new bcoin.HTTPWallet({ 
    id, 
    token,
    network: 'testnet',
    db: 'leveldb'
});

(async () => {
  await httpWallet.open();
  const wallet = await httpWallet.getInfo();
  console.log(wallet);
  httpWallet.close();
})();
```

Individual wallets have their own api keys, referred to internally as "tokens" (a 32 byte hash - calculated as `HASH256(m/44'->ec-private-key|tokenDepth)`).

A wallet is always created with a corresponding token. When using api endpoints
for a specific wallet, the token must be sent back in the query string or json
body.

e.g. To get information from a wallet that requires a token

`GET /wallet/primary/tx/:hash?token=977fbb8d212a1e78c7ce9dfda4ff3d7cc8bcd20c4ccf85d2c9c84bbef6c88b3c`


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
const id = 'test';

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
	"id": "test",
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
		...
	}
}
```

Get wallet info by ID.

### HTTP Request
`GET /wallet/:id`

Parameters | Description
---------- | -----------
id <br> _string_ | named id of the wallet whose info you would like to retrieve

## Wallet Rescan

> Response Body:

```json
    {"success": true}
```

Initiates a blockchain rescan for the walletdb. Wallets will be rolled back to the specified height (transactions above this height will be unconfirmed).

### Example HTTP Request
`POST /rescan?height=100000`



## HTTP Request 

`POST /wallet/_admin/resend` 

##POST /wallet/_admin/backup 

Safely backup the wallet database to specified path (creates a clone of the database).

## HTTP Request 

`POST /wallet/_admin/backup` 

##GET /wallet/_admin/wallets 

List all wallet IDs. Returns an array of strings.

## HTTP Request 

`GET /wallet/_admin/wallets` 

##GET /wallet/:id 

> Sample response: 

```json 
{
  "network": "regtest",
  "wid": 1,
  "id": "primary",
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
}```

Get wallet info by ID.

## HTTP Request 

`GET /wallet/:id` 

##GET /wallet/:id/master 

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
}```

Get wallet master HD key. This is normally censored in the wallet info route. The provided api key must have admin access.

## HTTP Request 

`GET /wallet/:id/master` 

##PUT /wallet/:id 

> Sample response: 

```json 
{
  "network": "regtest",
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
}```

Create a new wallet with a specified ID.

## HTTP Request 

`PUT /wallet/:id` 

##GET /wallet/:id/account 

> Sample response: 

```json 
[
  "default"
]```

List all account names (array indicies map directly to bip44 account indicies).

## HTTP Request 

`GET /wallet/:id/account` 

##GET /wallet/:id/account/:account 

> Sample response: 

```json 
{
  "wid": 1,
  "id": "primary",
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
}```

Get account info.

## HTTP Request 

`GET /wallet/:id/account/:account` 

##PUT /wallet/:id/account/:name 

> Sample response: 

```json 
{
  "wid": 1,
  "id": "primary",
  "name": "menace",
  "initialized": true,
  "witness": false,
  "watchOnly": false,
  "type": "pubkeyhash",
  "m": 1,
  "n": 1,
  "accountIndex": 1,
  "receiveDepth": 1,
  "changeDepth": 1,
  "nestedDepth": 0,
  "lookahead": 10,
  "receiveAddress": "mg7b3H3ZCHx3fwvUf8gaRHwcgsL7WdJQXv",
  "nestedAddress": null,
  "changeAddress": "mkYtQFpxDcqutMJtyzKNFPnn97zhft56wH",
  "accountKey": "tpubDC5u44zLNUVo55dtQsJRsbQgeNfrp8ctxVEdDqDQtR7ES9XG5h1SGhkv2HCuKA2RZysaFzkuy5bgxF9egvG5BJgapWwbYMU4BJ1SeSj916G",
  "keys": []
}```

Create account with specified account name.

## HTTP Request 

`PUT /wallet/:id/account/:name` 

##POST /wallet/:id/passphrase 

Change wallet passphrase. Encrypt if unencrypted.

## HTTP Request 

`POST /wallet/:id/passphrase` 

##POST /wallet/:id/unlock 

Derive the AES key from passphrase and hold it in memory for a specified number
of seconds. Note: During this time, account creation and signing of
transactions will not require a passphrase.

## HTTP Request 

`POST /wallet/:id/unlock` 

##POST /wallet/:id/lock 

If unlock was called, zero the derived AES key and revert to normal behavior.

## HTTP Request 

`POST /wallet/:id/lock` 

##POST /wallet/:id/import 

A rescan will be required to see any transaction history associated with the
key.

## HTTP Request 

`POST /wallet/:id/import` 

##POST /wallet/:id/retoken 

> Sample response: 

```json 
{
  "token": "2d04e217877f15ba920d02c24c6c18f4d39df92f3ae851bec37f0ade063244b2"
}```

Note: if you happen to lose the returned token, you will not be able to
access the wallet.

## HTTP Request 

`POST /wallet/:id/retoken` 

##POST /wallet/:id/send 

> Sample response: 

```json 
{
  "wid": 1,
  "id": "primary",
  "hash": "0de09025e68b78e13f5543f46a9516fa37fcc06409bf03eda0e85ed34018f822",
  "height": -1,
  "block": null,
  "time": 0,
  "mtime": 1486685530,
  "date": "2017-02-10T00:12:10Z",
  "index": -1,
  "size": 226,
  "virtualSize": 226,
  "fee": "0.0000454",
  "rate": "0.00020088",
  "confirmations": 0,
  "inputs": [
    {
      "value": "50.0",
      "address": "n4UANJbj2ZWy1kgt9g45XFGp57FQvqR8ZJ",
      "path": {
        "name": "default",
        "account": 0,
        "change": false,
        "derivation": "m/0'/0/0"
      }
    }
  ],
  "outputs": [
    {
      "value": "5.0",
      "address": "mu5Puppq4Es3mibRskMwoGjoZujHCFRwGS",
      "path": {
        "name": "default",
        "account": 0,
        "change": false,
        "derivation": "m/0'/0/7"
      }
    },
    {
      "value": "44.9999546",
      "address": "n3nFYgQR2mrLwC3X66xHNsx4UqhS3rkSnY",
      "path": {
        "name": "default",
        "account": 0,
        "change": true,
        "derivation": "m/0'/1/0"
      }
    }
  ],
  "tx": "0100000001c5b23b4348b7fa801f498465e06f9e80cf2f61eead23028de14b67fa78df3716000000006b483045022100d3d4d945cdd85f0ed561ae8da549cb083ab37d82fcff5b9023f0cce608f1dffe02206fc1fd866575061dcfa3d12f691c0a2f03041bdb75a36cd72098be096ff62a810121021b018b19426faa59fdda7f57e68c42d925752454d9ea0d6feed8ac186074a4bcffffffff020065cd1d000000001976a91494bc546a84c481fbd30d34cfeeb58fd20d8a59bc88ac447b380c010000001976a914f4376876aa04f36fc71a2618878986504e40ef9c88ac00000000"
}```

Create, sign, and send a transaction.

## HTTP Request 

`POST /wallet/:id/send` 

##POST /wallet/:id/create 

> Sample response: 

```json 
{
  "hash": "0799a1d3ebfd108d2578a60e1b685350d42e1ef4d5cd326f99b8bf794c81ed17",
  "witnessHash": "0799a1d3ebfd108d2578a60e1b685350d42e1ef4d5cd326f99b8bf794c81ed17",
  "fee": "0.0000454",
  "rate": "0.00020088",
  "mtime": 1486686322,
  "version": 1,
  "flag": 1,
  "inputs": [
    {
      "prevout": {
        "hash": "6dd8dfa9b425a4126061a1032bc6ff6e208b75ee09d0aac089d105dcf972465a",
        "index": 0
      },
      "script": "483045022100e7f1d57e47cd8a28b7c27e015b291f3fd43a6eb0c051a4b65d8697b5133c29f5022020cada0f62a32aecd473f606780b2aef3fd9cbd44cfd5e9e3d9fe6eee32912df012102272dae7ff2302597cb785fd95529da6c07e32946e65ead419291258aa7b17871",
      "witness": "00",
      "sequence": 4294967295,
      "coin": {
        "version": 1,
        "height": 2,
        "value": "50.0",
        "script": "76a9149621fb4fc6e2e48538f56928f79bef968bf17ac888ac",
        "address": "muCnMvAoUFZXzuao4oy3vQJFcUntax53wE",
        "coinbase": true
      }
    }
  ],
  "outputs": [
    {
      "value": "5.0",
      "script": "76a91494bc546a84c481fbd30d34cfeeb58fd20d8a59bc88ac",
      "address": "mu5Puppq4Es3mibRskMwoGjoZujHCFRwGS"
    },
    {
      "value": "44.9999546",
      "script": "76a91458e0ea4c9722e7d079ebadb75cb3d8c16dafae7188ac",
      "address": "mocuCKUU6oS6n1yyx81Au71An252SUYwDW"
    }
  ],
  "locktime": 0
}```

Create and template a transaction (useful for multisig).
Do not broadcast or add to wallet.

## HTTP Request 

`POST /wallet/:id/create` 

##POST /wallet/:id/sign 

Sign a templated transaction (useful for multisig).

## HTTP Request 

`POST /wallet/:id/sign` 

##POST /wallet/:id/zap 

Remove all pending transactions older than a specified age. Returns array of txids.

## HTTP Request 

`POST /wallet/:id/zap` 

##DEL /wallet/:id/tx/:hash 

Remove a pending transaction.

## HTTP Request 

`DEL /wallet/:id/tx/:hash` 

##GET /wallet/:id/block 

List all block heights which contain any wallet txs.

## HTTP Request 

`GET /wallet/:id/block` 

##GET /wallet/:id/block/:height 

> Sample response: 

```json 
{
  "hash": "39864ce2f29635638bbdc3e943b3a182040fdceb6679fa3dabc8c827e05ff6a7",
  "height": 3,
  "time": 1485471341,
  "hashes": [
    "dd1a110edcdcbb3110a1cbe0a545e4b0a7813ffa5e77df691478205191dad66f"
  ]
}```

Get block info by height. Contains a list of all wallet txs included in the
block.

## HTTP Request 

`GET /wallet/:id/block/:height` 

##PUT /wallet/:id/shared-key 

Add a shared xpubkey to wallet. Must be a multisig wallet.

## HTTP Request 

`PUT /wallet/:id/shared-key` 

##DEL /wallet/:id/shared-key 

Remove shared xpubkey from wallet if present.

## HTTP Request 

`DEL /wallet/:id/shared-key` 

##GET /wallet/:id/key/:address 

Get wallet key by address.

## HTTP Request 

`GET /wallet/:id/key/:address` 

##GET /wallet/:id/wif/:address 

Get wallet private key (WIF format) by address.

## HTTP Request 

`GET /wallet/:id/wif/:address` 

##POST /wallet/:id/address 

> Sample response: 

```json 
{
  "network": "regtest",
  "wid": 1,
  "id": "primary",
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
}```

Derive new receiving address for account.

## HTTP Request 

`POST /wallet/:id/address` 

##POST /wallet/:id/change 

Derive new change address for account.

## HTTP Request 

`POST /wallet/:id/change` 

##POST /wallet/:id/nested 

Derive new nested p2sh receiving address for account.

## HTTP Request 

`POST /wallet/:id/nested` 

##GET /wallet/:id/balance 

> Sample response: 

```json 
{
  "wid": 1,
  "id": "primary",
  "account": -1,
  "unconfirmed": "8149.9999546",
  "confirmed": "8150.0"
}```

Get wallet or account balance.

## HTTP Request 

`GET /wallet/:id/balance` 

##GET /wallet/:id/coin 

List all wallet coins available.

## HTTP Request 

`GET /wallet/:id/coin` 

##GET /wallet/:id/locked 

> Sample response: 

```json 
[{"hash":"dd1a110edcdcbb3110a1cbe0a545e4b0a7813ffa5e77df691478205191dad66f","index":0}]```

Get all locked outpoints.

## HTTP Request 

`GET /wallet/:id/locked` 

##PUT /wallet/:id/locked/:hash/:index 

Lock outpoints.

## HTTP Request 

`PUT /wallet/:id/locked/:hash/:index` 

##DEL /wallet/:id/locked/:hash/:index 

Unlock outpoints.

## HTTP Request 

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
]```

Get wallet coins.

## HTTP Request 

`GET /wallet/:id/coin/:hash/:index` 

##GET /wallet/:id/tx/history 

Get wallet TX history. Returns array of tx details.

## HTTP Request 

`GET /wallet/:id/tx/history` 

##GET /wallet/:id/tx/unconfirmed 

Get pending wallet transactions. Returns array of tx details.

## HTTP Request 

`GET /wallet/:id/tx/unconfirmed` 

##GET /wallet/:id/tx/range 

Get range of wallet transactions by timestamp. Returns array of tx details.

## HTTP Request 

`GET /wallet/:id/tx/range` 

##GET /wallet/:id/tx/last 

Get last N wallet transactions.

## HTTP Request 

`GET /wallet/:id/tx/last` 

##GET /wallet/:id/tx/:hash 

Get wallet transaction details.

## HTTP Request 

`GET /wallet/:id/tx/:hash` 
