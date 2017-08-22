# Wallet Accounts
## Account Object
> An account object looks like this:

```json
  {
    "wid": 1,
    "id": "test",
    "name": "default",
    "initialized": true,
    "witness": false,
    "watchOnly": false,
    "type": "pubkeyhash",
    "m": 1,
    "n": 1,
    "accountIndex": 0,
    "receiveDepth": 8,
    "changeDepth": 1,
    "nestedDepth": 0,
    "lookahead": 10,
    "receiveAddress": "mu5Puppq4Es3mibRskMwoGjoZujHCFRwGS",
    "nestedAddress": null,
    "changeAddress": "n3nFYgQR2mrLwC3X66xHNsx4UqhS3rkSnY",
    "accountKey": "tpubDC5u44zLNUVo2gPVdqCbtX644PKccH5VZB3nqUgeCiwKoi6BQZGtr5d6hhougcD6PqjszsbR3xHrQ5k8yTbUt64aSthWuNdGi7zSwfGVuxc",
    "keys": []
  } 

```
Represents a BIP44 Account belonging to a Wallet.
Note that this object does not enforce locks. Any method that does a write is internal API only and will lead to race conditions if used elsewhere.

From the [BIP44 Specification](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki):

<p>
    <code>
        This level splits the key space into independent user identities, so the wallet never mixes the coins across different accounts.
    </code>
</p>
<p>
    <code>
        Users can use these accounts to organize the funds in the same fashion as bank accounts; for donation purposes (where all addresses are considered public), for saving purposes, for common expenses etc.
    </code>
 </p>
 <p>
    <code>
        Accounts are numbered from index 0 in sequentially increasing manner. This number is used as child index in BIP32 derivation.
    </code>
</p>
<p>
    <code>
        Hardened derivation is used at this level.
    </code>
</p>

## Get Wallet Account List

```shell--vars
id='test' 
```

```shell--cli
bcoin cli wallet account list --id=$id
```

```javascript
const client = new bcoin.http.Client();
(async () => {
  const accountInfo = await client.getAccounts(id);
  console.log(accountInfo);
})();
```

> Sample response: 

```json 
[
  "default"
]
```

List all account names (array indicies map directly to bip44 account indicies) associated with a specific wallet id.

### HTTP Request 

`GET /wallet/:id/account`


Parameters | Description
---------- | -----------
id <br> _string_ | id of wallet you would like to retrieve the account list for

<aside class="notice">
Note that command defaults to primary (default) wallet if no wallet id is passed
</aside>

## Get Account Information
```javascript
let id, account;
```

```shell--vars
id='test'
account='default'
```

```shell--curl
curl $url/wallet/$id/account/$account
```

```shell--cli
bcoin cli wallet --id=$id account get $account
```

```javascript
const client = new bcoin.http.Client();
(async () => {
  const accountInfo = await client.getAccount(id, account);
  console.log(accountInfo);
})();
```

> Sample response: 

```json 
{
  "wid": 1,
  "id": "test",
  "name": "default",
  "initialized": true,
  "witness": false,
  "watchOnly": false,
  "type": "pubkeyhash",
  "m": 1,
  "n": 1,
  "accountIndex": 0,
  "receiveDepth": 8,
  "changeDepth": 1,
  "nestedDepth": 0,
  "lookahead": 10,
  "receiveAddress": "mu5Puppq4Es3mibRskMwoGjoZujHCFRwGS",
  "nestedAddress": null,
  "changeAddress": "n3nFYgQR2mrLwC3X66xHNsx4UqhS3rkSnY",
  "accountKey": "tpubDC5u44zLNUVo2gPVdqCbtX644PKccH5VZB3nqUgeCiwKoi6BQZGtr5d6hhougcD6PqjszsbR3xHrQ5k8yTbUt64aSthWuNdGi7zSwfGVuxc",
  "keys": []
}
```

Get account info.

### HTTP Request 

`GET /wallet/:id/account/:account` 

Parameters | Description
---------- | -----------
id <br> _string_ | id of wallet you would like to query
account <br> _string_ | id of account you would to retrieve information for

## Create new wallet account

```javascript
let id, name;
```

```shell--vars
id='test'
name='menace'
```

```shell--cli
bcoin cli wallet --id=$id account create $name --type=multisig
```

```shell--curl
curl $url/wallet/$id/account/$name \
    -X PUT
    --data '{"type": "multisig"}'
```

```javascript
const client = new bcoin.http.Client();
const options = {type: 'multisig'};// options are the same as available wallet options

(async () => {
  const account = await client.createAccount(name, options);
  console.log(account);
});
```

> Sample response: 

```json 
{
  "wid": 1,
  "id": "test",
  "name": "menace",
  "initialized": true,
  "witness": false,
  "watchOnly": false,
  "type": "multisig",
  "m": 1,
  "n": 1,
  "accountIndex": 1,
  "receiveDepth": 1,
  "changeDepth": 1,
  "nestedDepth": 0,
  "lookahead": 10,
  "receiveAddress": "mg7b3H3ZCHx3fwvUf8gaRHwcgsL7WdJQXv",
  "nestedAddress": null,
  "changeAddress": "mkYtQFpxDcqutMJtyzKNFPnn97zhft56wH",
  "accountKey": "tpubDC5u44zLNUVo55dtQsJRsbQgeNfrp8ctxVEdDqDQtR7ES9XG5h1SGhkv2HCuKA2RZysaFzkuy5bgxF9egvG5BJgapWwbYMU4BJ1SeSj916G",
  "keys": []
}
```

Create account with specified account name.

### HTTP Request 

`PUT /wallet/:id/account/:name` 