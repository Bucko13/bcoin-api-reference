# Wallet Transactions

## Delete Transaction
```javascript
let id, hash, passphrase;
```

```shell--vars
id="foo"
hash="2a22606ee555d2c26ec979f0c45cd2dc18c7177056189cb345989749fd58786"
passphrase="bar"
```

```shell--cli
# Not available in CLI
```

```shell--curl
curl $url/wallet/$id/tx/$hash \
  -X DELETE \
  --data '{"passphrase": "'$passphrase'"}'
```

```javascript
 // Not available in javascript wallet client.
```

Abandon single pending transaction. Confirmed transactions will throw an error.
`"TX not eligible"`

### HTTP Request

`DEL /wallet/:id/tx/:hash`

Paramters | Description
----------| --------------------
id <br> _string_ | id of wallet where the transaction is that you want to remove
hash <br> _string_ | hash of transaction you would like to remove.

## Get Wallet TX History

```javascript
let id;
```

```shell--vars
id='foo'
```

```shell--cli
bcoin cli wallet --id=$id history
```

```shell--curl
curl $url/wallet/$id/tx/history
```

```javascript
const httpWallet = new bcoin.http.Wallet({ id: id });
const account = 'default';

(async () => {
  const response = httpWallet.getHistory(account);
  console.log(response);
})();

```
> Sample Response

```json
[
  {
     "wid": 1,
     "id": "primary",
     "hash": "f5968051ce275d89b7a6b797eb6e6b081243ecf027872fc6949fae443e21b858",
     "height": -1,
     "block": null,
     "time": 0,
     "mtime": 1503690544,
     "date": "2017-08-25T19:49:04Z",
     "size": 226,
     "virtualSize": 226,
     "fee": 0,
     "rate": 0,
     "confirmations": 0,
     "inputs": [
       {
         "value": 0,
         "address": "mp2w1u4oqZnHDd1zDeAvCTX9B3SaFsUFQx",
         "path": null
       }
     ],
     "outputs": [
       {
         "value": 100000,
         "address": "myCkrhQbJwqM8wKi9YuhyTjN3pukNuWxZ9",
         "path": {
           "name": "default",
           "account": 0,
           "change": false,
           "derivation": "m/0'/0/3"
         }
       },
       {
         "value": 29790920,
         "address": "mqNm1rSYVqD23Aj6fkupApuSok9DNZAeBk",
         "path": null
       }
     ],
     "tx": "0100000001ef8a38cc946c57634c2db05fc298bf94f5c88829c5a6e2b0610fcc7b38a9264f010000006b483045022100e98db5ddb92686fe77bb44f86ce8bf6ff693c1a1fb2fb434c6eeed7cf5e7bed4022053dca3980a902ece82fb8e9e5204c26946893388e4663dbb71e78946f49dd0f90121024c4abc2a3683891b35c04e6d40a07ee78e7d86ad9d7a14265fe214fe84513676ffffffff02a0860100000000001976a914c2013ac1a5f6a9ae91f66e71bbfae4cc762c2ca988acc892c601000000001976a9146c2483bf52052e1125fc75dd77dad06d65b70a8288ac00000000"
   },
 ...
]
```

Get wallet TX history. Returns array of tx details.

### HTTP Request
`GET /wallet/:id/tx/history`

##GET /wallet/:id/tx/unconfirmed

Get pending wallet transactions. Returns array of tx details.

### HTTP Request

`GET /wallet/:id/tx/unconfirmed`

##GET /wallet/:id/tx/range

Get range of wallet transactions by timestamp. Returns array of tx details.

### HTTP Request

`GET /wallet/:id/tx/range`

##GET /wallet/:id/tx/last

Get last N wallet transactions.

### HTTP Request

`GET /wallet/:id/tx/last`

##GET /wallet/:id/tx/:hash

Get wallet transaction details.

### HTTP Request

`GET /wallet/:id/tx/:hash`