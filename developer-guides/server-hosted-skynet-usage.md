---
description: >-
  Add import example for using both nodejs-skynet and skynet-js? Can we just add
  tus to nodejs sdk to reduce complications?
---

# Server-Hosted Skynet Usage

> This is a draft version documenting ideas for using Skynet from outside of a browser context. We’re working to improve this tooling, so this information is subject to change.

## Overview

As of 07/12/2021, the tooling for Skynet outside of the browser is somewhat disjointed. Hopefully this guide will walk you through some of the main considerations of interacting with Skynet from a server-side context.

This is a work-in-progess, so please reach out if you run into any issues.

> If you are working with us as a partner, please reach out to [daniel@siasky.net](mailto:daniel@siasky.net) \(Discord dghelm\#8125\) for a promo code for a free elevated tier account.

## File Uploads

`skynet-js` currently only supports browser uploads. You will want to upload files using one of our other SDK or CLI tools \(ie [nodejs-skynet](https://github.com/SkynetLabs/nodejs-skynet)\), even though they don’t support the full feature-set of Skynet.

{% hint style="info" %}
The node SDK has been updated to support large file uploads. The tus protocol will automatically be used for any file &gt;40MB.
{% endhint %}

## File Persistence

In a browser context, cookies containing encrypted JWTs are used for linking file uploads to your portal account. This is needed for pinning files &gt;90 days.

If your upload method does not support cookies and your file needs persisted longer than 90 days, you will want to upload your file then use your encrypted JSON Web Token \(located in the `skynet-jwt` cookie\) to “pin” files to your account.

### Using your JWT

When initializing `SkynetClient` you will want to pass along your JWT as follows:

```javascript
import { SkynetClient } from 'skynet-js';

const portal = 'https://siasky.net'

const client = new SkynetClient(portal, {customCookie: SKYNET_JWT});
```

Where the value of `SKYNET_JWT` is not committed to the repo and is in the format where the value begins with `skynet-jwt=`.

### Obtaining your JWT

1. First, login to your account at [https://account.siasky.net/](https://account.siasky.net/)
2. Press `F12` to open your developer tools, select the “Application” tab
3. In the bar at left, under “Storage”, find “[account.siasky.net](http://account.siasky.net/)” under “Cookies”
4. Locate the item named “skynet-jwt” and copy-paste its value, but make sure the `string` is prefixed with `skynet-jwt=`.

![](https://i.imgur.com/Ncjojqr.png)

### “Re-pining” a Skylink using your Account

Once the file is uploaded, you need to the [Siasky.net](http://siasky.net/) portal that you wish to associate your account with the given file and commit to “pinning” the file. Your account will then treat the skylink as being one you uploaded.

After initializing the client with a custom cookie: `client.pinSkylink(skylink);`

## Cookie / JWT Expiration

JWTs expire after 720 hours and we have no tooling to detect if your JWT is expired. We don’t currently support API Keys, so please reach out if you want a longer expiration time on your JWT.

## Skylink v2 Creation

One common use-case for server-side applications is to upload files that are pointed to using a v2 Skylink, so that the data can be consumed using a consistent URL.

Here is a code example:

```javascript
import { SkynetClient, genKeyPairFromSeed } from 'skynet-js';
import { SKYNET_JWT, SECRET_SEED } from './consts';

const portal = 'https://siasky.net';
const client = new SkynetClient(portal, { customCookie: SKYNET_JWT })

// Setup Keys for Read/Write of Mutable Data
const { privateKey, publicKey } = genKeyPairFromSeed( SECRET_SEED );
const dataKey = 'nameForRegistryEntry';

// skylink to point to with v2 Skylink
const skylink = "sia://MABdWWku6YETM2zooGCjQi26Rs4a6Hb74q26i-vMMcximQ";

// Set Registry Entry to point at our Skylink
await client.db.setDataLink(privateKey, dataKey, skylink);

// Get the Skylink V2 that represents the registry entry
const skylinkV2 = await client.registry.getEntryLink(publicKey, dataKey);

// Get the URL for the skylink, at `siasky.net`
const publicUrl = await client.getSkylinkUrl(skylinkV2);
```

