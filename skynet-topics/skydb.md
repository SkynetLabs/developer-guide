# SkyDB

## Introduction

SkyDB provides an easy interface for accessing, creating, and updating mutable data on Skynet. It was the first variety of mutable data storage on Skynet and inspired much of the thinking behind more robust data types like [MySky Files](mysky-and-dacs/mysky-files.md#introduction).

SkyDB is a key-value store database building on Skynet's registry and immutable skyfiles. By using the registry, users and applications are able to write JSON contents to an immutable file that is accessed using their public key and a "data key" they assign the file to. Writes can only be made using the user's private key \(paired with the data key\).

SkyDB uses the registry for the mutable piece of its storage. The registry has limited data capacity, so instead of recording the JSON data directly, it stores a pointer to a skylink that contains the actual JSON data. Per byte, the registry is expensive to store and access when compared to typical Skyfiles, which is why they are heavily constrained. Developers should take this into consideration when designing their applications.

## Using SkyDB

The primary way to access SkyDB is by calling `client.db.getJSON` and `client.db.setJSON`, for more information see the [SkyDB documentation](https://siasky.net/docs/#skydb) for skynet-js.

{% hint style="info" %}
skynet-js is the only SDK maintained by Skynet Labs that supports SkyDB, and it currently only works in a browser/client-side context.
{% endhint %}

## Considerations for using SkyDB

You do not want to hard-code a private key for SkyDB into your application. It is trivial for an attacker to compromise your data. If you want to use SkyDB with private keys provided by the user, [MySky ](mysky-and-dacs/#introduction)may be a more robust and secure approach.

## Further Reading

{% embed url="https://blog.sia.tech/skydb-a-mutable-database-for-the-decentralized-web-7170beeaa985" %}

{% embed url="https://blog.sia.tech/mutating-the-immutable-behind-the-scenes-of-skydb-137df8105c17" %}



