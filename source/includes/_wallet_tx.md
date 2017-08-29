# Wallet Transactions

## Send a transaction

```javascript
let id, rate, value, address;
```

```shell--vars
id='foo'
rate=100
value=.3
address="moTyiK7aExe2v3hFJ9BCsYooTziX15PGuA"
```

```shell--cli
bcoin cli wallet send --id=$id --value=$value --address=$address
```

```shell--curl
curl $url/wallet/$id/send \
  -X POST \
  --data '{
    "rate":'$rate',
    "outputs":[
      {"address":"'$address'", "value":'$value'}
    ] 
  }'
```

```javascript
const httpWallet = bcoin.http.wallet({ id: id });
const options = {
  rate: rate,
  outputs: [{ value: value, address; address }]
};

(async () => {
  const tx = await httpWallet.send(options);
  console.log(tx);
})();
```

> Sample response: 

```json 
{
{
  "wid": 13,
  "id": "foo",
  "hash": "c2da22cafcd076ea3db74bb2e3cf50f030e5240aa5daf4f778fb4954a866b41c",
  "height": -1,
  "block": null,
  "time": 0,
  "mtime": 1503364758,
  "date": "2017-08-22T01:19:18Z",
  "size": 225,
  "virtualSize": 225,
  "fee": 22,
  "rate": 100,
  "confirmations": 0,
  "inputs": [
    {
      "value": 59991393,
      "address": "mgChJ3wXDqRns7Y6UhjXCyxeuZZJoQNj7c",
      "path": {
        "name": "default",
        "account": 0,
        "change": true,
        "derivation": "m/0'/1/5"
      }
    }
  ],
  "outputs": [
    {
      "value": 10000000,
      "script": "76a9143f4f69730dcb175c830b94226ae13f89bef969c488ac",
      "address": "mmHhzmwiUzorZLYhFH9fhrfFTAHGhx1biN"
    },
    {
      "value": 30000000,
      "script": "76a9143f4f69730dcb175c830b94226ae13f89bef969c488ac",
      "address": "mmHhzmwiUzorZLYhFH9fhrfFTAHGhx1biN"
    }

  ],
  "tx": "01000000015a9b8fa3fb300a29e9cde6464f49882228862b8e333792fea35ad15536383417010000006a47304402202df28a6fe24dc26b016acee539e137b9502009f57ae6988d468d203e770339f202203b6ab4cc020493061db2d405b2799af2b872d3395f5798616fc51e59f304d5cd0121028986f0724eb55b66bba72985212b95a2c5487631e411dc9cc5348a4531928129ffffffff02e8030000000000001976a9144462dc0989942e38474616dc104e46486c5744ee88ac63619303000000001976a9149affd314659d5ce9fa815fde4e82c879d1ea41d188ac00000000"
}

```

Create, sign, and send a transaction.

### HTTP Request 

`POST /wallet/:id/send` 

### Post Paramaters
Parameter | Description
--------- | ------------------
rate <br> _int_ | the rate for transaction fees. Denominated in satoshis per kb
outputs <br> _array_ | An array of outputs to send for the transaction
smart <br> _bool_ | whether or not to estimate a smart fee based on current mempool
account <br> _string_ | account to use for transaction
passphrase <br> _string_ | passphrase to unlock the account


### Output object
Property | Description
--------- | -----------
value <br> _int_ | Value to send in bitcoin (as of beta-1.14)
address <br> _string_ | destination address for transaction 

## Create a Transaction
```javascript
let id, rate, value, address;
```

```shell--vars
id='foo'
rate=100
value=.3
address="moTyiK7aExe2v3hFJ9BCsYooTziX15PGuA"
```

```shell--cli
bcoin cli wallet mktx --id=$id --rate=$rate --value=$value --address=$address
```

```shell--curl
curl $url/wallet/$id/create \
  -X POST \
  --data '{
    "rate":'$rate',
    "outputs":[
      {"address":"'$address'", "value":'$value'}
    ] 
  }'
```

```javascript
const httpWallet = new bcoin.http.wallet({ id: id });
const outputs: [{ value: value, address; address }]
const options = {
  rate: rate,
};

(async () => {
  const tx = await httpWallet.createTX(options, outputs);
  console.log(tx);
})();
```

> Sample response: 

```json 
{
  "hash": "0799a1d3ebfd108d2578a60e1b685350d42e1ef4d5cd326f99b8bf794c81ed17",
  "witnessHash": "0799a1d3ebfd108d2578a60e1b685350d42e1ef4d5cd326f99b8bf794c81ed17",
  "fee": "0.0000454",
  "rate": "0.00020088",
  "mtime": 1486686322,
  "version": 1,
  "flag": 1,
  "inputs": [
    {
      "prevout": {
        "hash": "6dd8dfa9b425a4126061a1032bc6ff6e208b75ee09d0aac089d105dcf972465a",
        "index": 0
      },
      "script": "483045022100e7f1d57e47cd8a28b7c27e015b291f3fd43a6eb0c051a4b65d8697b5133c29f5022020cada0f62a32aecd473f606780b2aef3fd9cbd44cfd5e9e3d9fe6eee32912df012102272dae7ff2302597cb785fd95529da6c07e32946e65ead419291258aa7b17871",
      "witness": "00",
      "sequence": 4294967295,
      "coin": {
        "version": 1,
        "height": 2,
        "value": "50.0",
        "script": "76a9149621fb4fc6e2e48538f56928f79bef968bf17ac888ac",
        "address": "muCnMvAoUFZXzuao4oy3vQJFcUntax53wE",
        "coinbase": true
      }
    }
  ],
  "outputs": [
    {
      "value": 10000000,
      "script": "76a9143f4f69730dcb175c830b94226ae13f89bef969c488ac",
      "address": "mmHhzmwiUzorZLYhFH9fhrfFTAHGhx1biN"
    },
    {
      "value": 30000000,
      "script": "76a9143f4f69730dcb175c830b94226ae13f89bef969c488ac",
      "address": "moTyiK7aExe2v3hFJ9BCsYooTziX15PGuA"
    }
  ],
  "locktime": 0
}
```

Create and template a transaction (useful for multisig).
Do not broadcast or add to wallet.

### HTTP Request 

`POST /wallet/:id/create` 

### Post Paramters
Paramter | Description
--------- | ----------------
rate <br> _int_ | the rate for transaction fees. Denominated in satoshis per kb
outputs <br> _array_ | An array of outputs to send for the transaction
smart <br> _bool_ | whether or not to estimate a smart fee based on current mempool
passphrase <br> _string_ | passphrase to unlock the account


### Output object
Property | Description
--------- | -----------
value <br> _int_ | Value to send in bitcoin (as of beta-1.14)
address <br> _string_ | destination address for transaction 



## Sign Transaction
```javascript
let id, tx, passphrase;
```

```shell--vars
id="foo"
passphrase="bar"
tx="01000000010d72c6b2582c2b2e625d29dd5ad89209de7e2600ab12a1a8e05813c28b703d2c000000006b483045022100af93a8761ad22af858c5bc4e68b5991eac017dcddd933cf125553ec0b83eb8f30220373a4d8ee331ac4c3975718e2a789f873af0520ddbd2db18957cdf488ccd4ee301210215a9110e2a9b293c332c28d69f88081aa2a949fde67e35a13fbe19410994ffd9ffffffff0280969800000000001976a9143f4f69730dcb175c830b94226ae13f89bef969c488ac80c3c901000000001976a9143f4f69730dcb175c830b94226ae13f89bef969c488ac00000000"
```

```shell--cli
bcoin cli wallet sign --id=$id --passphrase=$passphrase --tx=$tx 
```

```shell--curl
curl $url/wallet/$id/sign \
  -X POST \
  --data '{"tx": "'$tx'", "passphrase":"'$passphrase'"}'
```

```javascript
const httpWallet = new bcoin.http.Wallet({ id: id });
const options = { passphrase: passphrase };
(async () => {
  const signedTx = await httpWallet.sign(tx, options);
  console.log(signedTx);
})();
```

> Sample Output

```json
{
  "hash": "2a22606ee555d2c26ec979f0c45cd2dc18c7177056189cb345989749fd587868",
  "witnessHash": "2a22606ee555d2c26ec979f0c45cd2dc18c7177056189cb345989749fd587868",
  "fee": 10000000,
  "rate": 44247787,
  "mtime": 1503683721,
  "version": 1,
  "inputs": [
    {
      "prevout": {
        "hash": "2c3d708bc21358e0a8a112ab00267ede0992d85add295d622e2b2c58b2c6720d",
        "index": 0
      },
      "script": "483045022100af93a8761ad22af858c5bc4e68b5991eac017dcddd933cf125553ec0b83eb8f30220373a4d8ee331ac4c3975718e2a789f873af0520ddbd2db18957cdf488ccd4ee301210215a9110e2a9b293c332c28d69f88081aa2a949fde67e35a13fbe19410994ffd9",
      "witness": "00",
      "sequence": 4294967295,
      "coin": {
        "version": 1,
        "height": 1179720,
        "value": 50000000,
        "script": "76a9145730f139d833e3af30ccfb7c4e253ff4bab5de9888ac",
        "address": "moTyiK7aExe2v3hFJ9BCsYooTziX15PGuA",
        "coinbase": false
      }
    }
  ],
  "outputs": [
    {
      "value": 10000000,
      "script": "76a9143f4f69730dcb175c830b94226ae13f89bef969c488ac",
      "address": "mmHhzmwiUzorZLYhFH9fhrfFTAHGhx1biN"
    },
    {
      "value": 30000000,
      "script": "76a9143f4f69730dcb175c830b94226ae13f89bef969c488ac",
      "address": "mmHhzmwiUzorZLYhFH9fhrfFTAHGhx1biN"
    }
  ],
  "locktime": 0,
  "hex": "01000000010d72c6b2582c2b2e625d29dd5ad89209de7e2600ab12a1a8e05813c28b703d2c000000006b483045022100af93a8761ad22af858c5bc4e68b5991eac017dcddd933cf125553ec0b83eb8f30220373a4d8ee331ac4c3975718e2a789f873af0520ddbd2db18957cdf488ccd4ee301210215a9110e2a9b293c332c28d69f88081aa2a949fde67e35a13fbe19410994ffd9ffffffff0280969800000000001976a9143f4f69730dcb175c830b94226ae13f89bef969c488ac80c3c901000000001976a9143f4f69730dcb175c830b94226ae13f89bef969c488ac00000000"
}
```

Sign a templated transaction (useful for multisig).

### HTTP Request 

`POST /wallet/:id/sign` 

### Post Paramters
Parameter | Description
----------| -----------------
tx <br> _string_ | the hex of the transaction you would like to sign
passphrase <br> _string_ | passphrase to unlock the wallet

## Zap Transactions

```javascript
let id, age, account;
```

```shell--vars
id="foo"
account="baz"
age=259200 # 72 hours
```

```shell--cli
bcoin cli wallet zap --id=$id account=$account age=$age
```

```shell--curl
curl $url/wallet/$id/zap \
  -X POST \
  --data '{
            "account": "'$account'",
            "age": "'$age'"
          }'
```

```javascript
const httpWallet = new bcoin.http.wallet({ id: id });

(async () => {
  const response = httpWallet.zap(account, age);
  console.log(response);
})();
```

> Sample Response


```json
{
  "success": true
}
```

Remove all pending transactions older than a specified age.

### HTTP Request 

`POST /wallet/:id/zap?age=3600` 

### Post Parameters
Paramaters | Description
----------- | -------------
account <br> _string_ or _number_ | account to zap from
age <br> _number_ | age threshold to zap up to (unix time)

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

##GET /wallet/:id/tx/history 

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