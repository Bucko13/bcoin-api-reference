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