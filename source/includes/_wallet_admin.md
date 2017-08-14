# Wallet Admin API
The _admin namespace exists to differentiate administrative level tasks on the wallet API that you probably don't want to expose to individual wallets. 

### Sample Request:
`/wallet/_admin/[TARGET_ACTION]`

<aside class="notice">
Replace `[TARGET_ACTION]` with one of the available actions listed below
</aside>

## Wallet Rescan

> Response Body:

```json
    {"success": true}
```

Initiates a blockchain rescan for the walletdb. Wallets will be rolled back to the specified height (transactions above this height will be unconfirmed).

### Example HTTP Request
`POST /wallet/_admin/rescan`


## Wallet Resend

Rebroadcast all pending transactions in all wallets.

### HTTP Request 

`POST /wallet/_admin/resend` 

##Wallet Backup

Safely backup the wallet database to specified path (creates a clone of the database).

### HTTP Request 

`POST /wallet/_admin/backup` 

## List all Wallets

List all wallet IDs. Returns an array of strings.

### HTTP Request 

`GET /wallet/_admin/wallets`  