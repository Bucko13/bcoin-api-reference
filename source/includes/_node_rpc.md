# Node RPC Calls

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{ "method": "methodname", "params": [...] "id": "some-id" }'
```

```shell--cli
bcoin cli rpc params...
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('MethodName', [ ...params ]);

})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON structured like this:

```json
{"result": resultObject ,"error": errorObject, "id": passedID}
```


Bcoin rpc calls mimic Bitcoin Core's RPC.
This is documentation how to use it with `bcoin`.

RPC Calls are accepted at:
`POST /`

### POST Parameters (JSON)
Parameter | Description
--------- | -----------
method  | Name of the RPC call
params  | Parameters accepted by method
id      | Will be returned with the response (Shouldn't be object)

## stop

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{ "method": "stop" }'
```

```shell--cli
bcoin cli rpc stop
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('stop');

})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON structured like this:

```json
{"result":"Stopping.","error":null,"id":null}
```

Stops the running node.

### Method and Params
Parameter | Description
--------- | -----------
None. |
