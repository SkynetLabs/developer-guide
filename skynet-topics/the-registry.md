# The Registry

## Introduction

The Registry is a high-performance key-value store for small amounts of data. Registry entries are the foundation for mutable data on Skynet and are stored on Sia hosts. Registry entries can be accessed using a public key and data key and updated using the private key and data key associated with the registry entry.

## Working with the Registry

Although the registry can be [read](https://sia.tech/docs/#skynet-registry-get) and [written](https://sia.tech/docs/#skynet-registry-post) to using the `skyd` API, `skynet-js` simplifies the process of reading or creating and signing registry entries, as seen in [the documentation](https://siasky.net/docs/#registry).

The registry is intended to be a low-level resource for advanced developers, and byte arrays (`Uint8Array`) are used for storing. The max size for custom data is 70 bytes, which is enough for 2 skylinks.

### Key Pairs

The keys used in the registry use `ed25519` key pairs and signatures, so a key pair will be needed for interacting with the registry directly. When using MySky, this will be handled by MySky.

```javascript
import { genKeyPairFromSeed } from "skynet-js";

const { publicKey, privateKey } = genKeyPairFromSeed("this seed should be fairly long for security");
```

{% hint style="info" %}
Although `skynet-js` doesn't fully support nodejs, the registry methods are fully compatible and can be used in Nodejs projects.
{% endhint %}

### Getting an entry

A `SkynetClient` is used to read a registry entry. The client automatically verifies the signature of the entry and returns the full entry along with the signature.

```javascript
const dataKey = "foo";
const { entry, signature } = await client.registry.getEntry(publicKey, dataKey);
const { dataKey, data, revision } = entry;

// dataKey = "foo"
// revision = 0n
// data = Uint8Array(3) encoding "bar"
```

### Setting an entry

Writing an entry works similarly, as the SkynetClient simplifies the signature process. You'll likely want to fetch the current registry entry the increment the current revision number.

```javascript
const dataKey = "foo";
const data = Uint8Array.from([98, 97, 114]); // "bar" character codes
const revision = 1n; // BigInt used, higher than current revision
const entry = { dataKey, data, revision };

await client.registry.setEntry(privateKey, entry);
```

### Considerations

Registry entries are very fast and require only a single HTTP request to read or write, but, from the portal perspective, they are also expensive. That is why they get paired with Skyfiles for storing larger amounts of data. Excessive usage may result in rate-limiting by the portal for users that are not logged in with a portal account.

{% hint style="info" %}
The Registry might seem limited by itself -- be sure to see MySky Files, SkyDB and Resolver Skylinks to see their full power.
{% endhint %}

## Using the Registry outside of skynet-js

### Reading with API

Registry entries are read using `GET` requests to the `skynet/registry` endpoint on web portals or [skyd](https://sia.tech/docs/#skynet-registry-get) instances.

The URL uses a hash of the `dataKey` so the above example can be [accessed here](https://siasky.net/skynet/registry?publickey=ed25519%3A6759d5ced2c0c9b4f308c19ddca7304291754323589e7cf9e417351744ae9465\&datakey=056f1ef8df2086e5aa284c1331cea86662be52b2a2deca83fc1683dc91be11a3\&timeout=5).

```javascript
{
"data": "41414476715f70436231316a304b5041385643537539494a525162433347326647383046634e65495a336d654e77",
"revision": 0,
"datakey": "056f1ef8df2086e5aa284c1331cea86662be52b2a2deca83fc1683dc91be11a3",
"publickey": {
    "algorithm": "ed25519",
    "key": "Z1nVztLAybTzCMGd3KcwQpF1QyNYnnz55Bc1F0SulGU="
},
"signature": "6ce8890f7ea0340f93073886f1531c36ecd336ec63be6334a8912f027298e4837b557da75621693931765727a0fcf58ecfad53f9ca1ecbcda983d3204058b804",
"type": 1
}
```

#### Writing to the Registry

To write to the registry, you'll want to repeat the steps utilized in the `setEntry` method. ([See code here.](https://github.com/SkynetLabs/skynet-js/blob/a3d880d768be0ad0fe1be4d29123eb86ce788931/src/registry.ts#L312)) You'll want to create the entry, sign it with a a private key, then make `POST` request to the registry endpoint with form data that looks like the code below.

```javascript
{"publickey":{"algorithm":"ed25519","key":[103,89,213,206,210,192,201,180,243,8,193,157,220,167,48,66,145,117,67,35,88,158,124,249,228,23,53,23,68,174,148,101]},"datakey":"056f1ef8df2086e5aa284c1331cea86662be52b2a2deca83fc1683dc91be11a3","revision":1,"data":[98,97,114],"signature":[130,59,249,195,59,104,17,81,93,161,29,81,93,196,87,7,15,170,203,170,62,155,174,212,8,110,118,251,187,75,93,124,203,147,165,249,21,208,42,166,143,132,56,66,199,113,253,133,149,121,154,81,104,233,103,230,200,95,32,87,179,127,59,5,101,117,137,186,119,97,6,156,112,35,70,202,57,35,143,130,156,226,88,169,111,233,245,170,164,105,77,32,52,134,78,34]}
```

#### Other libraries which support updating the registry

Registry interaction has support in various community libraries including redsolver's [Skynet SDK for Dart](https://github.com/redsolver/skynet), and PowerLoom's [py-skydb](https://github.com/PowerLoom/py-skydb) (where the registry is used implicitly).

## Full Description

{% embed url="https://blog.sia.tech/the-host-registry-building-dynamic-content-on-skynet-ade72ba6f30b" %}
Chris has written a full desciption of the registry, reproduced below.
{% endembed %}

### The Host Registry: Building Dynamic Content on Skynet

In the early days of the internet, websites were simple static documents. Every user accessing a resource would see the same content. It wasn’t until after websites became more dynamic and users could log in and fill the page with custom content that the internet really began to thrive. Similarly, Skynet started as a platform where people could create decentralized applications called Skapps by uploading a bundle of static websites. A user would then receive an immutable Skylink to a static Skapp that they could use to access it. Users could freely create and share content over a decentralized and trustless platform. But of course, we couldn’t stop at static content.

#### Host Registry

To fully unlock the potential of Skapps, we had to introduce addressable dynamic content in a decentralized and trustless way. That’s what we designed the Host Registry for—a new type of high-performance key-value store for small amounts of data on hosts. It allows for setting so-called registry entries with a max size of 256 bytes, of which around 100 bytes can be set. The remainder is overhead, such as a cryptographic signature by the entry owner, but we will get to that later. That small size allows them to be stored in RAM and as a result, they can be rapidly updated and read.

The key part of the entries is a combination of a public key and a data key where the public key is an actual cryptographic public key, and the data key is a 32-byte hash. The cryptographic key guarantees that only an entity with the corresponding private key can update an entry since every entry needs to be signed for an update. The data key allows a single public key to set multiple different entries.

#### A quick example — Note To Self

To better understand registry entries, let’s look at an example. [Note-To-Self](https://note-to-self.hns.siasky.net) is a minimal Skapp that allows a user to set a note, open the Skapp from a different device, and read that note. The Skapp requires users to enter a secret passphrase. This passphrase is used to derive a public-private key pair. So every unique password will have a different key pair.

Upon submitting a note, the note is uploaded to the portal, and a Skylink is returned to the Skapp. But this Skylink is not enough because the next time we access Note-To-Self from a different browser, it won’t know the Skylink. To work around that, it creates a registry entry that contains the Skylink as the payload. It uses the private key derived from our passphrase to sign it and a constant data key that we know when setting the entry. Any data key will do such as the hash of the string “note”. In this case, the data key is hardcoded into the Skapp, but that’s not a requirement.

The next time we access the Skapp, it derives our public and private keys from our passphrase again. Then it tells the portal that we would like to fetch the registry entry for our public key and the data key used to set it. The portal will return the registry entry, followed by the Skapp extracting the Skylink from it to download the note we uploaded before presenting it to the user.

The most notable aspect of this is the fact that it works completely decentralized and trustless manner. We don’t need to trust the portal since the browser can verify the signature on the registry entry. We can also set a note on one device using a specific portal and a second later read that note on a different device using a different portal. Users could also share a passphrase to leave notes for each other like a grocery list.

#### Registry Entries

As mentioned before, a complete and signed registry entry can be up to 256 bytes large. That is the entry’s size on the host’s disk because some additional information is required when persisting it. The host persists the following data for each entry.

* Data Key: 32 bytes
* Public Key: 33 bytes
* Expiry: 4 bytes
* Data Length: 1 byte
* Data: 0-113 bytes (usually exactly 34 bytes for a Skylink)
* Revision: 8 bytes
* Type: 1 byte
* Signature: 64 bytes

We already know what the data key and public key are. The Expiry is used by the host to know when it is safe to prune the entry from the map (more on that later). The data is the payload an entity can set. The revision is used to order entry updates. The type serves to distinguish between potentially different entry types in the future, and the signature guarantees the verifiability of the entry’s integrity when fetching it.

When setting a registry entry, multiple hosts on the network are updated. Similar to how data is uploaded redundantly, we want to ensure the entry doesn’t get lost. Since file contracts do not cover the registry, we need a deterministic way for hosts to determine which of two presented and signed registry entries should overwrite the other. This is done using two rules.

1. The entry with the higher revision number has priority
2. If the revision numbers match, compare the hashes of the entries. The one that resembles the larger number wins.

The first rule is quite straightforward. If you want to update an entry, raise its revision number and broadcast it to hosts. They will look at the number and know to swap the old entry out for a new one.

The second rule is important whenever a host is presented with two entries with the same revision number. In that case, we still want the network to converge on a single known truth. It’s also useful in case an entry should be updated after reaching its maximum value. Maybe due to the need for deleting its value.

When fetching an entry, a portal asks the whole network for a public key and data key pair. It is expected that responses may vary in revision number, but that’s not an issue. A portal will apply the same rules as the hosts while waiting for hosts to respond with their latest known entries. After all hosts have responded or after enough time has passed, the portal will respond with the best entry from all hosts.

#### Example

Let’s take a look at a quick example. A Skapp requests a registry entry from a portal for a user’s public key and data key. The portal then asks the network for that entry. Due to the distributed nature of the network, four different versions A to D are returned from four different hosts in the following order.

* **A**: Revision 100, hash 0xA, invalid signature
* **B**: Revision 1, hash 0xB, valid signature
* **C**: Revision 2, hash 0xC, valid signature
* **D**: Revision 2, hash 0xD, valid signature

The portal will reject **A** right away due to the invalid signature. Even though it has the highest revision. It then looks at **B** and considers it the best candidate for now. After receiving **C**, it rejects **B** since **C** has a higher revision number. Finally, it receives **D** which has the same revision number as **C** but since 0xD is greater than 0xC, it rejects **C** and returns **D** to the user.

#### New Incentives

We mentioned already that for the sake of performance, registry entries are not covered by file contracts. That means a host can delete them without being penalized through a loss of collateral. To prevent a host from doing so, we make use of a different incentive.

Registry entries are tiny amounts of data that are frequently accessed and updated compared to regular data in file contracts. They are not intended for long-term storage of data that is rarely or never accessed. So the percentage of registry entries that are uploaded but never downloaded is slim compared to regular uploads to and downloads from Skynet.

The fact that entries are so small allows portals to overpay for uploading and retrieving them. In fact setting an entry costs 5 years of storing 256 bytes on the host, and retrieving an entry costs 10 years of storing 256 bytes. As a result, a host doesn’t gain a lot from deleting entries because just setting it once creates a minuscule amount of revenue for the host. On the other hand, the host can create a good flow of revenue by serving the entry. This means the host is incentivized to keep entries since the opportunity cost of keeping them is much higher than benefit of freeing up disk space and the risk of being identified as a malicious host.

Of course, registry entries are also uploaded at a much higher redundancy than regular files to minimize the risk of losing access to an entry. Additionally, portals keep track of revisions they have already seen hosts serve. That way, hosts can quickly be identified as liars when they cannot serve an entry that they have served before.

#### Conclusion

The host registry allows for dynamically loading content within Skapps in a completely trustless and decentralized way. 999 out of 1000 updates to and reads from the registry finish in under 200ms on average across servers run by Skynet Labs while the best portals can do it in under 100ms. It enables developers to build a new generation of powerful Skapps, achieving feature parity with Twitter, Dropbox, and others. But in a completely decentralized, user-empowering way.

If you are interested in trying it out yourselves, check out our [developer section](https://siasky.net/developers) which contains guides on how you can get started right away.

Also, stay tuned for our next blog post on how we used the Host Registry to introduce a new feature to Skylinks. Resolver Skylinks allow for a Skylink to point to different content without changing the actual link. Until now, Skylinks could only reference an immutable file on Skynet. With the new Resolver Skylinks, a Skylink can now point to SkyDB data, a MySky File, or other mutable data. It could even point to another Skylink associated with immutable data.

We can’t wait for the huge possibilities for a more flexible DWeb via Resolver Skylinks!
