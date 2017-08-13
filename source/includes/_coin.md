# Coin
Getting coin information via APi.

*Coin stands for UTXO*

<aside class="info">
You need to enable <code>index-tx</code> and <code>index-address</code> in order
to lookup coins by transaction hashes and addresses, respectively.
</aside>


## Get coin by Outpoint

```javascript
let hash, index;
```

```shell--vars
hash='c13039f53247f9ca14206da079bcf738d91bc60e251ac9ebaba9ea9a862d9092';
index=0;
```

```shell--curl
curl $url/coin/$hash/$index
```

```shell--cli
bcoin cli coin $hash $index
```

```javascript
const client = new bcoin.http.Client({
  network: 'testnet',
  db: 'leveldb'
});

(async () => {
  await client.open();

  const coin = await client.getCoin(hash, index);

  console.log(coin);

  await client.close();
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON structured like this:

```json
{
  "version": 1,
  "height": 100019,
  "value": 144709200,
  "script": "76a9148fc80aadf127f6f92b6ed33404b110f851c8eca188ac",
  "address": "mtdCZdNYny2U7met3umk47SoA7HMZGfsa2",
  "coinbase": false,
  "hash": "c13039f53247f9ca14206da079bcf738d91bc60e251ac9ebaba9ea9a862d9092",
  "index": 0
}
```

Get coin by outpoint (hash and index). Returns coin in bcoin coin json format.

### HTTP Request

### URL Parameters
Parameter | Description
--------- | -----------



## Get coin by address

```javascript
```

```shell--vars
```

```shell--curl
```

```shell--cli
```

```javascript
```

> The above command returns JSON structured like this:

```json
```

Get coin object by address

### HTTP Request

### URL Parameters
Parameter | Description
--------- | -----------



## Get coins by addresses

```javascript
```

```shell--vars
```

```shell--curl
```

```shell--cli
```

```javascript
```

> The above command returns JSON structured like this:

```json
```

Get coins by addresses,
returns array of coin objects.

### HTTP Request

### POST Parameters (JSON)
Parameter | Description
--------- | -----------
