## getwork

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "getwork",
    "params": []
  }'
```

```shell--cli
bcoin cli rpc getwork
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('getwork');

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON "result" like this:

```json
{
  "data": "2000000082c18ff1d76f61dccbf37d5eca24d252c009af5f622ce08700000580000000009f447a03a1df9ae8f1ee24f0cf5ab69f1b321071f0e57344f2c61b922c12b8335994bdb51a0ffff000000000000000800000000000000000000000000000000000000000000000000000000000000000000000000000000080020000",
  "target": "0000000000000000000000000000000000000000000000f0ff0f000000000000",
  "height": 1178782
}
```

Returns hashing work to be solved by miner.
Or submits solved block.

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
1 | data | | Data to be submitted to the network.

## getworklp

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "getworklp",
    "params": []
  }'
```

```shell--cli
# Because there's request timeout set on CLI http requests.
# you need to send custom request with different timeout.
```

```javascript
// Because there's request timeout set on rpc.execute,
// you need to send custom request with different timeout.
```

> The above command returns JSON "result" like this:

```json
{
  "data": "2000000082c18ff1d76f61dccbf37d5eca24d252c009af5f622ce08700000580000000009f447a03a1df9ae8f1ee24f0cf5ab69f1b321071f0e57344f2c61b922c12b8335994bdb51a0ffff000000000000000800000000000000000000000000000000000000000000000000000000000000000000000000000000080020000",
  "target": "0000000000000000000000000000000000000000000000f0ff0f000000000000",
  "height": 1178782
}
```

Returns new work, whenever new TX is received in the mempool or
new block has been discovered. So miner can restart mining on new data.

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

> The above command returns JSON "result" like this:

```json
453742051556.55084
```

Returns the estimated current or historical network hashes per second, based on last `blocks`.

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
1 | blocks | 120 | Number of blocks to lookup.
2 | height | 1 | Starting height for calculations.



## getmininginfo

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "getmininginfo",
    "params": []
  }'
```

```shell--cli
bcoin cli rpc getmininginfo
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('getmininginfo', []);

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON "result" like this:

```json
{
  "blocks": 1178765,
  "currentblocksize": 0,
  "currentblockweight": 0,
  "currentblocktx": 0,
  "difficulty": 0,
  "errors": "",
  "genproclimit": 0,
  "networkhashps": 10802664115772.53,
  "pooledtx": 27,
  "testnet": true,
  "chain": "test",
  "generate": false
}
```

Returns mining info, if you're running one.

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
