# Handshake & HNS Names

## Introduction

Handshake is a naming protocol that’s backward-compatible with the existing DNS system and has a deep integration in Skynet.

Please see the [Handshake Names](https://support.siasky.net/key-concepts/handshake-names) article in the Skynet Guide for an overview of Handshake Names, and how to get started with them.

{% hint style="info" %}
For step-by-step instructions of deploying with Handshake, see [Setting up a Handshake Name](../developer-guides/setting-up-a-handshake-name.md).
{% endhint %}

## How Skynet Uses Handshake

Skynet uses Handshake as a human-readable naming layer. We do not resolve on their DNS **A** records, but rather just look for a **TXT** entry containing a skylink. Skynet Web Portals run full Handshake nodes and do not rely on any centralized services to obtain information about the blockchain.

Because we only rely on blockchain records, we do not support subdomains, secondary name servers, or any records you might set in the "Namebase nameserver DNS records" section on Namebase.io.

## Accepted Formats

![Handshake DNS Using a Skylink](../.gitbook/assets/image%20%289%29.png)

When setting a **TXT** record for your domain, use a skylink format starting with `sia://` — this can be a Skylink directly referencing your web app, or a resolver skylink that can be updated to point at other skylinks. Updating a resolver skylink will propagate much faster than modifying your DNS records on the Handshake blockchain, a process which can take several hours. See [Resolver Skylinks](hns-names.md) for more info.

{% hint style="danger" %}
Notice these formats differ from DNSLink. We are not using traditional DNS resolution, and DNSLink requires records on subdomains to function properly.
{% endhint %}

{% hint style="warning" %}
We still support setting `skyns://` values for the **TXT** record, but recommend migrating to resolver skylinks. Since `skynet-js 4.0-beta`we no longer support the writing to the registry with the format used by `skyns`.

If you must use it, the URI format is `skyns://<public-key>/<hashed-data-key>` where the referenced registry entry points to the Skylink you wish to resolve using data encoding from `skynet-js 3.0`.
{% endhint %}

## Testing an HNS Name

To determine where an HNS name is set to resolve, make a GET request to `/skynet/hnsres/[hnsName]`

An example can be seen here, or try using the widget below, which takes advantage of the `skynet-js` method `client.resolveHns(hnsName)`

{% embed url="https://codesandbox.io/s/skynet-guide-widgets-jp5wt?codemirror=0&view=preview&fontsize=12&hidenavigation=1&theme=light&hidedevtools=1&initialpath=%2F%23%2Fhns-lookup" caption="Type Your HNS Name to Where a Web Portal Resolves It" %}

## Hosting at an HNS Name

If you want to use your TLD name for hosting a site, [dLinks](https://www.namebase.io/dlinks) lets you setup a profile page that is deployed to and hosted on Siasky.net. You can use the DNS settings it automatically sets up to point to any skylink or resolver skylink.

This [Namebase documentation](https://docs.namebase.io/guides-1/namebase-record-assistant#example-skylink-on-bare-tld) shows how to manually set the DNS records in the Namebase nameserver DNS records to use their Skynet resolver. Use `_contenthash` and a skylink for **TXT** records and `sia.namebase.io.` as the value of an **ALIAS** record on the `@` root or a **CNAME** record on your subdomain.

See the below example for [dghelm/](http://dghelm.hns.to/) and [mirror.dghelm/](http://mirror.dghelm.hns.to/) -- you can see these are distinct from [https://dghelm.hns.siasky.net/](https://dghelm.hns.siasky.net/) which is set in Blockchain DNS records section.

![A TLD and SLD each using Skynet-hosted Sites](../.gitbook/assets/image%20%285%29.png)

## Further Reading

{% embed url="https://blog.sia.tech/handshake-retrospective-after-the-first-year-c197e49749c9" caption="" %}

{% embed url="https://blog.sia.tech/dlinks-launches-on-skynet-d6883e6eff0a" caption="" %}

