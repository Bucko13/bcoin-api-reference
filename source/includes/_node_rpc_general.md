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

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON "result" like this:

```json
"Stopping."
```

Stops the running node.

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
None. |



## getinfo

```shell--curl
# Will return once all blocks are mined.
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "getinfo",
    "params": []
  }'
```

```shell--cli
bcoin cli rpc getinfo
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('getinfo');

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON "result" like this:

```json
{
  "version": "v1.0.0-beta.14",
  "protocolversion": 70015,
  "walletversion": 0,
  "balance": 0,
  "blocks": 1178980,
  "timeoffset": 0,
  "connections": 8,
  "proxy": "",
  "difficulty": 1048576,
  "testnet": true,
  "keypoololdest": 0,
  "keypoolsize": 0,
  "unlocked_until": 0,
  "paytxfee": 0.0002,
  "relayfee": 0.00001,
  "errors": ""
}
```

Returns general info


### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
None. |



## validateaddress

```javascript
let address;
```

```shell--vars
address='n34pHHSqsXJQwq9FXUsrfhmTghrVtN74yo';
```

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "validateaddress",
    "params": [ "'$address'" ]
  }'
```

```shell--cli
bcoin cli rpc validateaddress $address
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('validateaddress', [ address ]);

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON "result" like this:

```json
{
  "isvalid": true,
  "address": "n34pHHSqsXJQwq9FXUsrfhmTghrVtN74yo",
  "scriptPubKey": "76a914ec61435a3c8f0efee2ffafb8ddb4e1440d2db8d988ac",
  "ismine": false,
  "iswatchonly": false
}
```

Validates address.


### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
1 | address | Required | Address to validate



## createmultisig

```javascript
let nrequired, pubkey0, pubkey1, pubkey2;
```

```shell--vars
nrequired=2;
pubkey0='02b3280e779a7c849f9d6460e926097fe4b0f6280fa6fd038ce8e1236a4688c358';
pubkey1='021f1dbc575db95a44e016fe6ecf00231109e7799d9b1e007dbe8814017cf0d65c';
pubkey2='0315613667e3ebe065c0b8d86ae0443d97de56545bdf38c99a6ee584f300206d9a';
```

```shell--curl
curl $url/ \
  -H 'Content-Type: application/json' \
  -X POST \
  --data '{
    "method": "createmultisig",
    "params": [ "'$nrequired'", [ "'$pubkey0'", "'$pubkey1'", "'$pubkey2'" ] ]
  }'
```

```shell--cli
bcoin cli rpc createmultisig $nrequired '[ "'$pubkey0'", "'$pubkey1'", "'$pubkey2'" ]'
```

```javascript
const rpc = new bcoin.http.RPCClient({
  network: 'testnet'
});

(async () => {
  const res = await rpc.execute('createmultisig', [ nrequired, [ pubkey0, pubkey1, pubkey2 ] ]);

  console.log(res);
})().catch((err) => {
  console.error(err.stack);
});
```

> The above command returns JSON "result" like this:

```json
{
  "address": "2MzY9R5P1Wfy9aNdqyoH63K1EQZF7APuZ4S",
  "redeemScript": "5221021f1dbc575db95a44e016fe6ecf00231109e7799d9b1e007dbe8814017cf0d65c2102b3280e779a7c849f9d6460e926097fe4b0f6280fa6fd038ce8e1236a4688c358210315613667e3ebe065c0b8d86ae0443d97de56545bdf38c99a6ee584f300206d9a53ae"
}

```

create multisig address

### Params
N. | Name | Default |  Description
--------- | --------- | --------- | -----------
1 | nrequired | Required | Required number of approvals for spending
2 | keyArray  | Required | Array of public keys
