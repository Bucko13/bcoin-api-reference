## pruneblockchain

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "pruneblockchain",
    "params": []
  }'
```

```shell--cli
bcoin cli rpc pruneblockchain
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('pruneblockchain');

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

Prunes the blockchain, it will keep blocks specified in Network Configurations.

### Default Prune Options
Network | keepBlocks | pruneAfter
------- | -------    | -------
main    | 288        | 1000
testnet | 10000      | 1000
regtest | 10000      | 1000

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
None. |



## verifychain

Not implemented.

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
1 | checklevel | Required | Level of verification
1 | numblocks | Required | Number of blocks to verify



## invalidateblock

```javascript
let blockhash;
```

```shell--vars
blockhash='0000000000000dca8da883af9515dd90443d59139adbda3f9eeac1d18397fec3';
```

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "invalidateblock",
    "params": [ "'$blockhash'" ]
  }'
```

```shell--cli
bcoin cli rpc invalidateblock $blockhash
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('invalidateblock', [ blockhash ]);

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```


Invalidates the block in the chain.
It will rewind network to blockhash and invalidate it. 

It won't accept that block as valid
*Invalidation will work while running, restarting node will remove invalid block from list.*

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
1 | blockhash | Required | Block's hash



## reconsiderblock

```javascript
let blockhash;
```

```shell--vars
blockhash='0000000000000dca8da883af9515dd90443d59139adbda3f9eeac1d18397fec3';
```

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "reconsiderblock",
    "params": [ "'$blockhash'" ]
  }'
```

```shell--cli
bcoin cli rpc reconsiderblock $blockhash
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('reconsiderblock', [ blockhash ]);

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

This rpc command will remove block from invalid block set.

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
1 | blockhash | Required | Block's hash

## getnetworkhashps

```javascript
let blocks, height;
```

```shell--vars
blocks=120;
height=1000000;
```

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "getnetworkhashps",
    "params": [ "'$blocks'", "'$height'" ]
  }'
```

```shell--cli
bcoin cli rpc getnetworkhashps $blocks $height
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('getnetworkhashps', [ blocks, height ]);

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

Returns the estimated current or historical network hashes per second, based on last `blocks`.

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
1 | blocks | 120 | Number of blocks to lookup.
2 | height | 1 | Starting height for calculations.
