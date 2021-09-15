---
description: >-
  Web3 needs decentralized front-ends. Skynet's Homescreen lets users save
  copies of front-ends and lets developers worry less about keeping them online.
---

# Homescreen

## Introduction

[Homescreen](https://homescreen.hns.siasky.net/) allows users to save immutable versions of web app front-ends, without relying on centralized services. DeFi and Web3 especially need this for additional security and control over access to decentralized protocols and smart contracts.

To learn more about using Homescreen, see the [Skynet Guide article](https://support.siasky.net/key-concepts/homescreen) on Homescreen.

{% embed url="https://youtu.be/peyomCiTCpQ" %}

## How Homescreen Works

### Adding an App

Users can install apps in two ways:

1. Projects can place an "Add to Homescreen" button on their site or code repository. Users can click this to open Homescreen and choose to install the app.
2. Using the input bar at the top of Homescreen, users can add files or sites using skylinks, HNS names, or site URLs. We plan to also support ENS and DNSLink if they resolve to a webapp hosted on Skynet.

Both of these methods with pin the current, immutable skylink to the user's MySky storage. If present in the app's manifest file, the resolver skylink will also be retained for checking for future updates.

### Updating and Versioning an App

Updates are made available for apps by using [resolver skylinks](../../skynet-topics/resolver-skylinks.md). If a project updates where a resolver skylink resolves to, users will be able save the new underlying skylink as a "version" of the app.

Homescreen uses the resolver skylink in the `skylink` field of the app's [Manifest](adding-homescreen-support-to-an-app.md#3-configure-your-manifest-file). This skylink is also used for grouping saved versions and checking for updates. If no `skylink` field is found, the resolver skylink used when initially adding the app to Homescreen will be used. If the app has no resolver skylink associated with it, updates and versions are not available for the file or application.

### MySky & Homescreen Storage

In addition to pinning applications to Skynet, Homescreen uses decentralized storage for saving user data. Homescreen saves application data to [MySky Files](../../skynet-topics/mysky-and-dacs/mysky-files.md) which are fully controlled by the user's MySky seed.

## Integrating with Homescreen

For developers to add support for Homescreen, they need to do a few things:

* Confirm the application front-end supports a static deployment on Skynet \(Gatsby & full React Router support coming soon\)
* Assure APIs and web requests from your app only depend on decentralized protocols that will remain accessible even with future front-end updates. For web3, this usually means interacting with MetaMask or a Skynet portal, not making HTTP requests to your centralized back-end server. 
* Add an "Add to Homescreen" button on your project's Releases and README that links to Homescreen

{% hint style="warning" %}
Sites using centralized APIs may still work with Homescreen, but breaking changes to an API might break older builds that users have saved to Homescreen. This is not considered to be a best practice.
{% endhint %}

{% page-ref page="adding-homescreen-support-to-an-app.md" %}



## Explore More

{% embed url="https://www.youtube.com/watch?v=jOSXlAVeY7g" caption="Skynet Labs CEO David Vorick introucing Homescreen at EthCC\[4\]. See 12:15." %}

{% embed url="https://homescreen.hns.siasky.net/" caption="Try Homescreen" %}

{% embed url="https://github.com/SkynetLabs/uniswap-interface" caption="Example Repo for showing Homescreen Support  " %}

