# Resolver Skylinks

## Introduction

Resolver skylinks are a special type of skylink that enables skylinks whose underlying data changes. When resolver skylinks are accessed on Skynet, they "resolve" to other skylinks whose data is returned to the requester.

Each resolver skylink references a specific [registry entry](the-registry.md) whose saved data points to another skylink.

## Usage

### Accessing Resolver Skylinks

You can use or access resolver skylinks in any context you'd use a skylink for immutable data. You can access them in a browser at `https://[portalUrl]/[skylink]/`or for web apps using `https://[base32-skylink].[portalUrl]/`.

You can also use them with [HNS DNS Records](../integrations/hns-names.md), [ENS content records](../integrations/ens-ethereum-name-service.md), and [DNSLink](../integrations/dnslink-and-domain-names.md).

### Creating & Updating Resolver Skylinks

Resolver skylinks encode the data needed to look up a registry entry and can be generated for any data type powered by a registry entry, including SkyDB entries and MySky Files.

#### skynet-js

Creating a registry skylink in `skynet-js` works just like interacting with the registry.

```javascript
import { getEntryLink, SkynetClient } from 'skynet-js';

const client = new SkynetClient(); 

// setup keys and skylink
const seed = "verySecureSeed";
const { publicKey, privateKey } = genKeyPairFromSeed(seed);
const dataKey = "myResolverSkylinkForDocument";
const skylink = "sia://XABvi7JtJbQSMAcDwnUnmp2FKDPjg8_tTTFP4BwMSxVdEg";

// set a registry entry to point at 'skylink'
await client.db.setDataLink(privateKey, dataKey, skylink);

// get the resolver skylink which references the registry entry
const resolverSkylink = getEntryLink(publicKey, dataKey)
```

{% hint style="info" %}
In the SDKs, we refer to `entryLink` and `dataLinks`. An "Entry Link" is the resolver skylink doing the pointing and whose URI refers to a specific registry entry. The "Data Link" is the pointed-to skylink stored in the registry entry. Data Links can be immutable content skylinks or resolver skylinks.
{% endhint %}

### Web Tools

Creating and updating resolver skylinks is also supported in a number of useful developer tools:

* [Deploy to Github Action](../developer-guides/deploy-github-actions.md)
* [Rift's DNS Section](https://riftapp.hns.siasky.net/#/dns)
* [sky-deploy.hns.siasky.net](https://sky-deploy.hns.siasky.net)
* Discord Bot: Message [`Skynet Bot#3101`](discord://discord.com/channels/@me/793635571649609778) with `.rs [key] [skylink]` \(do not use this for anything but testing\)

## Security & Verifiability

Requests for resolver skylinks include `skynet-proof` and `skynet-skylink` headers. Using these, an SDK can confirm that the webportal returned verifiable registry entries and that they point to the `skynet-skylink` which should represent the immutable skyfile served to the client.

## Full Description

> Chris has written a full desciption of the resolver skylinks, reproduced below.

### Encoding

Before understanding what’s different about Resolver Skylinks, one should first understand what a regular Skylink looks like. A Skylink has a raw size of 34 bytes. After base64 encoding it into a human-readable string, it’s 46 characters long. The 34 bytes are made up of two parts.

1. Bitfield \(2 bytes\) — contains version, offset, and length
2. Merkle Root \(32 bytes\) — the Merkle root of the file’s base sector

We are not going into the details of the bitfield encoding in this post, but the bitfield essentially contains three pieces of information. The version of the link is 0 for regular Syklinks, and the offset and length are used to find the file’s metadata within its base sector. The base sector is the file's first sector, containing the necessary information to fetch a full Skyfile from the network. The Merkle root uniquely identifies the base sector and portals fetch the base sector from hosts with it.

Resolver Skylinks are similar to normal Skylinks. They are also 34 bytes large and consist of two parts as well.

1. Bitfield \(2 bytes\) — contains the version.
2. Registry Entry ID \(32 bytes\) — a hash that identifies the registry entry

The bitfield also contains a version, but instead of 0, it is set to 1. The remaining bits are currently not in use and are all 0s. The 32-byte Registry Entry ID \(RID\) is a descriptor for a registry entry in the Host Registry.

### **Resolving the Skylink**

As mentioned in the last part of this series, a registry entry can be uniquely identified by a public key and data key. Its integrity is also verified using the public key. Hosts make use of that fact and internally use the hash of the concatenated public key and data key as the index for storing registry entries. That means they can look the registry entries up in constant time using the hash.

As a result, a portal doesn’t need to tell the host about the public key and data key when requesting a registry entry. Instead, the portal can transmit the hash, a.k.a. RID. Of course, the portal still needs to verify the integrity of the entry, which it can’t without the public key. So the host returns the public key alongside the registry entry in that case. The portal then hashes the public key and data key to verify that the host returned the right entry. Afterward, it continues with the signature verification as usual.

So let’s summarize the above real quick. We have a Resolver Skylink, which contains a RID that portals can use to fetch an entry from the Host Registry, which contains the actual Skylink pointing to a file. So what can we do with that?

As you might have guessed, when a user tries to download a Resolver Skylink, the portal first checks the version to identify it as such. After that, it extracts the RID and asks the hosts for the appropriate registry entry. From that registry entry, the portal extracts the Skylink. Finally, it proceeds to download the Skylink as it would any other Skylink. The only difference is that it also returns the registry entry in the response header for the caller to verify that they actually received the right content.

At this point, some people might wonder: “But what if the Resolver Skylink resolves to another Resolver Skylink?”. In that case, the portal recursively resolves the link. The maximum depth of this recursion is 1, which means a Resolver Skylink can point to another Resolver Skylink, but the latter has to point to a regular Skylink.

