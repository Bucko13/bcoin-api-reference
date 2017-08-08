---
title: Bcoin API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - javascript

toc_footers:
  - <a href='http://bcoin.io/slack-signup.html'>Join us on slack</a>
  - <a href='http://bcoin.io/guides.html'>Browse the guides</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - node
  - coin
  - transaction
  - wallet

search: true
---

# Introduction

Welcome to the Bcoin API!

The default bcoin HTTP server listens on the standard RPC port (`8332` for main, `18333` for testnet, and `18555` default for simnet). It exposes a REST json api, as well as a JSON-RPC api.

# Authentication

## Auth
```shell
$ curl http://x:[api-key]@127.0.0.1:8332/
```

```javascript

```

> Make sure to replace `[api-key]` with your own key.

Auth is accomplished via HTTP Basic Auth, using your node's API key (passed via --api-key).

<aside class="notice">
You must replace <code>[api-key]</code> with your personal API key.
</aside>
