---
description: >-
  Add import example for using both nodejs-skynet and skynet-js? Can we just add
  tus to nodejs sdk to reduce complications?
---

# Server-Hosted Skynet Usage

## Overview

Most of this documentation assumes you're interacting with Skynet from a client-side, browser context, using [skynet-js](https://github.com/SkynetLabs/skynet-js). This guide will walk you through some of the main considerations of interacting with Skynet from a server-side context.

> If you are working with us as a partner, please reach out to [daniel@siasky.net](mailto:daniel@siasky.net) (Discord dghelm#8125) for a promo code for a free elevated tier account.

{% hint style="warning" %}
Previous versions of this page recommended mixing skynet-js and skynet-nodejs usage in a single project. We now suggest developers only use one or the other, depending on how their code will be run.
{% endhint %}

## Skynet SDK Capabilities

Currently, only our NodeJS SDK has all the features addressed in this article.

<table><thead><tr><th>SDK</th><th data-type="checkbox">Skyfile Upload</th><th data-type="checkbox">Skyfile Download</th><th data-type="checkbox">MySky + DACs</th><th data-type="checkbox">Large File Upload (tus)</th><th data-type="checkbox">Registry Access</th><th data-type="checkbox">Resolver Skylink Set</th><th data-type="checkbox">Portal Account</th><th data-type="checkbox">File Pinning</th></tr></thead><tbody><tr><td><a href="https://github.com/SkynetLabs/nodejs-skynet">skynet-nodejs</a></td><td>true</td><td>true</td><td>false</td><td>true</td><td>true</td><td>true</td><td>true</td><td>true</td></tr><tr><td><a href="https://github.com/SkynetLabs/python-skynet">python-skynet</a></td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td></tr><tr><td><a href="https://github.com/SkynetLabs/go-skynet">go-skynet</a></td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td><td>false</td></tr></tbody></table>

> Looking to contribute? Many of these features are thin abstractions over our HTTP APIs. Take a look at open issues in the [Python SDK](https://github.com/SkynetLabs/python-skynet/issues/6) and [Go SDK](https://github.com/SkynetLabs/go-skynet/issues).

## File Uploads

`skynet-js` currently only supports browser uploads. You will want to upload files using one of our other SDK or CLI tools (ie [@skynetlabs/skynet-nodejs](https://www.npmjs.com/package/@skynetlabs/skynet-nodejs)).

{% hint style="info" %}
The node SDK has been updated to support large file uploads in version 2.1.0. The tus protocol will automatically be used for any file >40MB.
{% endhint %}

## File Persistence

In a browser context, cookies containing encrypted JWTs are used for linking file uploads to your portal account. This is needed for pinning files >90 days.

If your upload method does not support cookies and your file needs to persist longer than 90 days, you will want to upload your file then use your encrypted JSON Web Token (located in the `skynet-jwt` cookie) to “pin” files to your account.

### Using your JWT with skynet-nodejs

To use your account JWT, you will specify a `customCookie` when you initialize your client.

```javascript
const { SkynetClient } = require('@skynetlabs/skynet-nodejs');

const client = new SkynetClient('https://siasky.net', {customCookie: SKYNET_JWT});
```

Do not commit the value of `SKYNET_JWT` to the repo and be sure the value is formatred to be begins with `skynet-jwt=`.

{% hint style="warning" %}
We do not suggest trying to set `customCookie` in `skynet-js` since browsers will not include this cookie header in browser requests.
{% endhint %}

### Obtaining your JWT

Currently, we don't yet support API keys for accounts, so you need to obtain the value for your JSON web token.

1. First, login to your account at [https://account.siasky.net/](https://account.siasky.net)
2. Press `F12` to open your developer tools, select the “Application” tab
3. In the bar at left, under “Storage”, find “[account.siasky.net](http://account.siasky.net)” under “Cookies”
4. Locate the item named “skynet-jwt” and copy-paste its value, but make sure the `string` is prefixed with `skynet-jwt=`.

![](https://i.imgur.com/Ncjojqr.png)

### Pin a Skylink using your Account

To pin a skylink, first initialize your Skynet Client to use your JWT. After initializing the client with a custom cookie, call `client.pinSkylink(skylink);`

## Cookie / JWT Expiration

JWTs expire after 720 hours and we have no tooling to detect if your JWT is expired. We don’t currently support API Keys, so please reach out to our team if you want a longer expiration time on your JWT.

## Resolver Skylink Creation

One common use-case for server-side applications is to upload files that are pointed to using a resolver skylink, so that the data can be consumed using a consistent URL.

Here is a code example:

```javascript
const { SkynetClient, genKeyPairFromSeed } = require("@skynetlabs/skynet-nodejs");
const { SKYNET_JWT, SECRET_SEED } = require './consts';

const portal = 'https://siasky.net';
const client = new SkynetClient(portal, { customCookie: SKYNET_JWT })

// Setup Keys for Read/Write of Mutable Data
const { privateKey, publicKey } = genKeyPairFromSeed( SECRET_SEED );
const dataKey = 'nameForRegistryEntry';

// skylink to point to with resolver skylink
const skylink = "sia://MABdWWku6YETM2zooGCjQi26Rs4a6Hb74q26i-vMMcximQ";

// Set Registry Entry to point at our Skylink
await client.db.setDataLink(privateKey, dataKey, skylink);

// Get the resolver skylink that represents the registry entry
const resolverSkylink = await client.registry.getEntryLink(publicKey, dataKey);

// Get the URL for the resolver skylink, at `siasky.net`
const resolverSkylinkUrl = await client.getSkylinkUrl(resolverSkylink);
```
