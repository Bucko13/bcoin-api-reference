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
