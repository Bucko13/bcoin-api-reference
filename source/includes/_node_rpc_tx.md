## gettxout

```javascript
let txhash, index, includemempool;
```

```shell--vars
txhash='28d65fdaf5334ffd29066d7076f056bb112baa4bb0842f6eaa06171c277b4e8c';
index=0;
includemempool=1;
```

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "gettxout",
    "params": [ "'$txhash'", "'$index'", "'$includemempool'" ]
  }'
```

```shell--cli
bcoin cli rpc gettxout $txhash $index $includemempool
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('gettxout', [ txhash, index, includemempool ]);

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON "result" like this:

```json
{
  "bestblock": "00000000000004f0fbf1b2290e8255bbd468640d747fd9d44a16e77d9e129a55",
  "confirmations": 1,
  "value": 1.01,
  "scriptPubKey": {
    "asm": "OP_HASH160 6a58967510cfd7e04987b245f73dbf62e8d3fdf8 OP_EQUAL",
    "hex": "a9146a58967510cfd7e04987b245f73dbf62e8d3fdf887",
    "type": "SCRIPTHASH",
    "reqSigs": 1,
    "addresses": [
      "2N2wXjoQbEQTKQuqYdkpHMp7rPpnpumYYqe"
    ]
  },
  "version": 1,
  "coinbase": false
}
```

Get outpoint of the transaction.

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
1 | txid | Required | Transaction hash
2 | index | Required | Index of the Outpoint tx.
3 | includemempool | true | Whether to include mempool transactions.



## gettxoutsetinfo

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "gettxoutsetinfo",
    "params": []
  }'
```

```shell--cli
bcoin cli rpc gettxoutsetinfo
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('gettxoutsetinfo');

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON "result" like this:

```json
{
  "height": 1178729,
  "bestblock": "00000000000004f0fbf1b2290e8255bbd468640d747fd9d44a16e77d9e129a55",
  "transactions": 14827318,
  "txouts": 17644185,
  "bytes_serialized": 0,
  "hash_serialized": 0,
  "total_amount": 20544080.67292757
}
```

Returns information about UTXO's from Chain.

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
None. |



