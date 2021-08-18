# Portal Accounts

## Introduction

Portal Accounts allows a user to authenticate themself to a portal, so that the portal can keep track of the data belonging to the user and keep it pinned to Skynet. Any Skynet portal can setup their own account parameters, but see the Skynet Guide's [Portal Accounts](https://support.siasky.net/siasky.net-accounts/portal-accounts) page for more info on how Siasky.net handles user account.

{% hint style="warning" %}
Portal accounts are completely distinct from MySky accounts. MySky accounts are decentralized, work across Skynet, accessed from any portal. A portal account is centralized and works with a single web portal.
{% endhint %}

## Using a Portal Account

For the most part, portal accounts work in the background, out of sight of the user or developer. Requests to the web portal will contain a cookie that is transporting an encrypted JSON Web Token \(JWT\) which authenticates requests.

For web applications, a user accesses the app through the portal they have an account with. Afterward, uploads and downloads automatically register as to the user for longer pinning and counting towards their storage quotas.

For server-side applications, the application itself will want a user account with a portal. Requests will need to contain the JWT to inform the portal to pin uploaded data.

For more information on using a portal account with server-side, see Server-Hosted Skynet Usage's [Using Your JWT](../developer-guides/server-hosted-skynet-usage.md#using-your-jwt).

## Account Tier Limits

Web portals can set limits for different user tiers. The limits for Siasky.net are published in our [Tier Comparison](https://support.siasky.net/siasky.net-accounts/sign-up-and-pick-a-tier#tier-comparison), and available with more details [defined in the code](https://github.com/SkynetLabs/skynet-accounts/blob/main/database/user.go#L54). Bandwidth is listed in bits/second, sizes are in bytes.

To find the limits of the account you're logged in with and confirm you are logged in to a portal account, visit [https://account.siasky.net/user/limits](https://account.siasky.net/user/limits). _Note: This uses a different cookie than siasky.net, so requests to this endpoint will not use the JWT described above._

## More Info

Skynet Accounts is an open-source service component in the web portal stack -- to learn more about how it is integrated, see the [SkynetLabs/skynet-accounts](https://github.com/SkynetLabs/skynet-accounts) repo.

