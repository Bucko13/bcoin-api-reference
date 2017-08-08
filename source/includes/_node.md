# Node

## JSON-RPC Requests 

```shell

```

```javascript

```

> The above command returns JSON structured like this:

```json
[
  
]
```
Route for JSON-RPC requests, most of which mimic the bitcoind RPC calls completely.


## Get server info

```shell

```

```javascript

```

> The above command returns JSON structured like this:

```json
{
  "version": "1.0.0-alpha",
  "network": "regtest",
  "chain": {
    "height": 177,
    "tip": "18037e4e4906a6c9580356612a42bb4127888354825f4232d811ef7e89af376a",
    "progress": 0.9935725241578355
  },
  "pool": {
    "host": "96.82.67.198",
    "port": 48444,
    "agent": "/bcoin:1.0.0-alpha/",
    "services": "1001",
    "outbound": 1,
    "inbound": 1
  },
  "mempool": {
    "tx": 1,
    "size": 2776
  },
  "time": {
    "uptime": 6,
    "system": 1486696228,
    "adjusted": 1486696228,
    "offset": 0
  },
  "memory": {
    "rss": 63,
    "jsHeap": 24,
    "jsHeapTotal": 34,
    "nativeHeap": 28
  }
}
```


## Get mempool snapshot


```shell

```

```javascript

```

Get mempool snapshot (array of json txs).


## Get block by hash or height

```shell

```

```javascript

```

## Broadcast transaction

```shell

```

```javascript

```


Broadcast a transaction by adding it to the node's mempool. If mempool verification fails, the node will still forcefully advertise and relay the transaction for the next 60 seconds.
