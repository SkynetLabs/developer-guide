# Migrating from SkyID to MySky

## What is SkyID?

SkyID is a community initiative \(lead by the incredible [DaWe](https://github.com/DaWe35/) 🙌\) with many parts. One part profile/identity management, one part cross-app decentralized login, and one part Javascript library to enable those other features and improve developer experience.

MySky replaces a portion of the decentralized login responsibilities of SkyID and hopes to deprecate the Javascript library.

### MySky for Login, Seed Management & getJSON/setJSON

MySky will still cover these use-cases, and will even support users choosing SkyID as their "seed provider" so they're still able to log in using their SkyID seed through MySky.

When using MySky, you app will not be handling or using any of the user's private keys. This is for their protection and to increase trust in the Skynet ecosystem as we create a truly interoperable data layer. Instead, when you ask to write to SkyDB, MySky will handle deriving the necessary keys and signing any entries. Read more in [MySky](migrating-from-skyid-to-mysky.md).

{% hint style="warning" %}
At the moment, MySky supports "Discoverable" and "Hidden" file types and uses different seed derivations for each. We do not yet support SkyID's seed derivation method, although it could be useful for data migration in the future. Please let us know if this is a feature you need.
{% endhint %}

### If you're using SkyID for &lt;script&gt; imports

If you're using SkyID for accessing Skynet without using a Javascript build tools or package managers, we are now releasing versioned, browser-ready `skynet-js` builds.

**Beta Release:**

```markup
<script src="https://skynet-js.hns.siasky.net/4.0-beta/index.js"></script>
```

### If you're using SkyID for encryption

We don't currently support encryption in `skynet-js` -- it's on our short-term roadmap, but you may need to still use the SkyID library to support encryption in your application.

