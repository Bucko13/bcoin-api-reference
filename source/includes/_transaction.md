# Transaction
Transction manipulations via API.

<aside class="info">
You need to enable <code>index-tx</code> and <code>index-address</code> in order
to lookup transactions by transaction hashes and addresses, repsectively.
</aside>

## Get tx by txhash

```javascript
let txhash;
```

```shell--vars
txhash='86150a141ebe5903a5d31e701698a01d598b81f099ea7577dad73033eab02ef9';
```

```shell--curl
curl $url/tx/$txhash
```

```shell--cli
bcoin cli tx $txhash
```

```javascript
const client = new bcoin.http.Client({
  network: 'testnet',
  db: 'leveldb'
});

(async () => {
  await client.open();

  const tx = await client.getTX(txhash);

  console.log(tx);

  await client.close();
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON structured like this:

```json
{
  "hash": "86150a141ebe5903a5d31e701698a01d598b81f099ea7577dad73033eab02ef9",
  "witnessHash": "86150a141ebe5903a5d31e701698a01d598b81f099ea7577dad73033eab02ef9",
  "fee": 50000,
  "rate": 220264,
  "mtime": 1501093478,
  "height": 502,
  "block": "00000000e0290b7c66227c7499692aac5437860ee912424bf8eea3a3883a4e37",
  "time": 1296746959,
  "index": 8,
  "version": 1,
  "flag": 1,
  "inputs": [
    {
      "prevout": {
        "hash": "0173a5d24d393127d5e6fc043ff1a00dafc6a2777143cb98a803a0b6e8cd02c7",
        "index": 0
      },
      "script": "493046022100be75ae6dbf9eab7656562136511501c83918ca28c5f96565ca1960b3dbb581b6022100d15692af456e8721fddeeb0d6df5d8a147afd8a3b2a39bbceae9b1bdfd53ade20121038e297cf2cf71c16592c36ca48f5b2a5bbb73e776e772079f4c695b12eec1a509",
      "witness": "00",
      "sequence": 4294967295,
      "coin": {
        "version": 1,
        "height": 502,
        "value": 4824247882,
        "script": "76a914bc7aad9746a0bc03ed9715f13c94e554df90b84688ac",
        "address": "mxhYHwYZdYh1AkLsUbEmU9ZGdvLfoNRdD6",
        "coinbase": false
      }
    }
  ],
  "outputs": [
    {
      "value": 4823922491,
      "script": "76a914682215dfa6912d88f55a1853414d516122fcc66988ac",
      "address": "mq1ZQJW1qPeNPNL83mpAWfNFPW9qwGDR2K"
    },
    {
      "value": 275391,
      "script": "76a914edb1dfaf6e0b39449da811275386edf2eb54baba88ac",
      "address": "n3BmXQPa1dKi3zEyCdCGNHTuE5GLdmw1Tr"
    }
  ],
  "locktime": 0
}
```

### HTTP Request
`GET /tx/:txhash`

### URL Parameters
Parameter | Description
--------- | -----------
:txhash | Hash of tx.


## Get tx by address
```javascript
let address;
```

```shell--vars
address='n3BmXQPa1dKi3zEyCdCGNHTuE5GLdmw1Tr';
```

```shell--curl
curl $url/tx/address/$address
```

```shell--cli
bcoin cli tx $address
```

```javascript
const client = new bcoin.http.Client({
  network: 'testnet',
  db: 'leveldb'
});

(async () => {
  await client.open();

  const txs = await client.getTXByAddress(address);

  console.log(txs);

  await client.close();
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON structured like this:

```json
[
  {
    "hash": "8351d991c5dfb49d534fcd28f56bb2d5b0d5f31f5c9e2e0711b5f86312a5abfe",
    "witnessHash": "8351d991c5dfb49d534fcd28f56bb2d5b0d5f31f5c9e2e0711b5f86312a5abfe",
    "fee": 50000,
    "rate": 129198,
    "mtime": 1501093478,
    "height": 467,
    "block": "00000000057c13f1fa6b30c6ec32284875781e31474a532e96739523d926a9e2",
    "time": 1296743253,
    "index": 20,
    "version": 1,
    "flag": 1,
    "inputs": [
      {
        "prevout": {
          "hash": "d4d30e49228051bffe1317a013c06ae53f5325302a03cdcacb3312d0998f512a",
          "index": 1
        },
        "script": "47304402201c29f13e8d817f2c2d1ea8b89d1d603677b86d0b4658f5d836bb16c56dc5dc3e02203e815b7ef739ba95c7bbbfdd63f38baa0806ad235f73c7df2492271e0b14ea43012103d50917ce22f377797a28c5e17e33000ea7d7d149d98b942d83f25ef2a223a8aa",
        "witness": "00",
        "sequence": 4294967295,
        "coin": {
          "version": 1,
          "height": 464,
          "value": 457110,
          "script": "76a914583d5f973c850ec26f8efa39dabf8fbe0fcbb59c88ac",
          "address": "moZXHEWiTwWyy1HSwvNANn9CKoCocPzDwm",
          "coinbase": false
        }
      },
      {
        "prevout": {
          "hash": "3d708378adc61ad2b6d623bfbb89df92e3c88f6c85f6e132796d5abefae8c587",
          "index": 1
        },
        "script": "0d17a6a8512d174b6b679c375091483045022100a3d133ccd4353c6dbcd9dc035c059b9b45f3c044644613e2311b8290bd02a3fb022026ae0af0adaea2fad2bc76d40d77fde3628031ee73c8e0e36343d5585e9d93f50121029f15918cd48f9e5cecfc1fccf1efc0c518110a6d6258cf14d0ee49a0fd88a535",
        "witness": "00",
        "sequence": 4294967295,
        "coin": {
          "version": 1,
          "height": 466,
          "value": 966270,
          "script": "76a914edb1dfaf6e0b39449da811275386edf2eb54baba88ac",
          "address": "n3BmXQPa1dKi3zEyCdCGNHTuE5GLdmw1Tr",
          "coinbase": false
        }
      }
    ],
    "outputs": [
      {
        "value": 1021229,
        "script": "76a91419eb536dc042d76454bea8dbec5ddc384e783e5a88ac",
        "address": "mht16aZhnsHivv3cDGuzgHGvoLFy8rNkg8"
      },
      {
        "value": 352151,
        "script": "76a914342e5d1f2eb9c6e99fda90c85ca05aa36616644c88ac",
        "address": "mkGrySSnxcqRbtPCisApj3zXCQVmUUWbf1"
      }
    ],
    "locktime": 0
  },
  ...
]
```

Returns transaction object by address

### HTTP Request
`GET /tx/address/:address`

### URL Parameters
Parameter | Description
--------- | -----------
:address | Bitcoin address.

## Get tx by addresses
