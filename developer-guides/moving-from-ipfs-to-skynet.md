# Moving from IPFS to Skynet

## Introduction

Many web3 developers are familiar with IPFS for decentralized storage. Skynet has many similarities and differences, and this page will hopefully give you a sense of how they compare if you're already familiar with IPFS. Many of the differences arise from IPFS being P2P and Skynet being an extension of a blockchain network.

The two technologies have many basic ideas in common:

* skylinks are immutable, content-addressable data just like CIDs
* skylinks can be used for features like [ENS support](../integrations/ens-ethereum-name-service.md#ens-support) or [DNSLink](../integrations/dnslink-and-domain-names.md#setting-dns-records), just like CIDs
* IPFS uses gateways to allow users to interact with the network from the web, while Skynet calls these webportals
* IPFS has `ipns` names that let you use a consistent name and update where it points. Skynet has [Resolver Skylinks](../skynet-topics/resolver-skylinks.md).
* both projects use the idea of "pinning" data to make it available on the network, although the exact mechanics differ for each project

Beyond these similarities, we'll cover various other use cases and points of comparison below in more depth.

{% hint style="info" %}
This article intends to educate IPFS users about Skynet. We are not trying to sell Skynet as a better technology for all use-cases, and if you see any mistakes, please file an issue or PR on the documentation's [Github repo](https://github.com/SkynetLabs/developer-guide).
{% endhint %}

## Use Cases

### Front-end Deployment on Skynet

Webapps built for IPFS can automatically deploy just by uploading the file to Skynet. See our [Getting Started](../getting-started.md#uploading-a-site-to-skynet) page on how to upload your build, or look into our [Deploy-to-Skynet Github Action](deploy-github-actions.md).

Although IPFS website hosting is limited by the frameworks that it can support, Skynet does not have these limitations. Skynet's [Framework-Flexible Routing](../skynet-topics/framework-flexible-routing.md) enables support for Gatsby, React-Router, and various other single-page application and static site frameworks.

### On-Chain Pointers to Off-Chain Data

One common usage of IPFS is for persisting data and media in web3 applications where it is too large \(costly\) to be written on-chain. CID pointers are stored on-chain and applications can make use of this pointer to resolve additional data off-chain in a way that still matches the immutability guarantees expected of many web3 applications and on-chain transactions. \(NFTs are a common example of this.\)

Skylinks work wonderfully for this purpose as well, and Skynet already has seen support from projects like [ENS Domains](https://ens.domains).

### File Sharing & Serving

Skynet allows for anonymous users to upload and download from Skynet webportals, without any sepcial software or cryptocurrency. This allows applications like [SkySend](https://skysend.hns.siasky.net/) to create applications without the boundaries of user signup that can work across any Skynet webportal. 

### Mutable Data and Application Data

In addition to its immutable data layer, Skynet has robust tooling for mutable data that is trustlessly accessible for read and write access in the browser without any special extension. This allows Skynet to be used for more than just decentralized file hosting, and expand its ecosystem to include rich web3 apps.

## Comparison

### Persisting Data on the Network

All Skynet data has high reliability and performance because the data is stored on the Sia network. Sia hosts are incentivized using the Siacoin cryptocurrency to keep data available and quickly accessible. Decentralization is at the heart of Skynet and it was designed from the ground up to be a performant, decentralized, and trustless ecosystem.

#### Vanilla IPFS

IPFS \(without Filecoin or centralized pinning services\) requires the uploader to keep their IPFS node online to provide availability on the network. Even if you do host the file, if other nodes do not choose to replicate your file, the requestor of your CID will only be able to access your file if their machine can connect to your machine.

On Skynet, a skylink refers to data that is stored across hosts on the Sia network, distributed across the globe. They are incentivized to keep files online through the contracts they hold with Skynet portals.

![Sia Host Distribution, curtesy of https://siastats.info/hosts\_map](../.gitbook/assets/image%20%2815%29.png)

#### IPFS + Filecoin

To ensure availability and persistence, IPFS gets paired with other tools.

Filecoin is Protocol Labs' blockchain solution for incentivizing a secondary layer of hosts to make data available long-term.

From [the IPFS Docs](https://docs.ipfs.io/concepts/persistence/#long-term-storage):

> Filecoin provides users with a dependable, long-term storage solution. However, there are some limitations to consider. The retrieval process is not always as fast as an IPFS pinning service, and the minimum file size accepted by a Filecoin storage provider can be several GiB. Also, the process for creating a storage deal may seem complicated to new users who aren't familiar with blockchain transactions or simply aren't comfortable working within a command line.

Skynet does not have these limitations and is built on a proven blockchain network that pre-dates Ethereum. Filecoin hosts also do not participate in the public IPFS network, so persisting files to Filecoin, even if it were performant enough for your use case, does not guarantee a user will be able to access your data with only its CID.

#### IPFS + Pinning Service

Another way that IPFS can be kept online is by using a pinning service. [Pinata](https://www.pinata.cloud/) and [Infura](https://infura.io/) are two better-known ones.

With a pinning service, a user uploads data or provides a CID to the pinning service. Then, the pinning service both a\) hosts the data, and b\) makes it available on IPFS. The user must have an account with the provider, who is both hosting the content and making it available to the rest of the network. The user is trusting in this relationship for data to remain hosted.

{% hint style="info" %}
 The user is trusting two business relationships. The relationship between the user and the pinning provider, and the relationship between the pinning provider and their underlying infrastructure provider.
{% endhint %}

With Skynet, a user typically has their data pinned by a webportal. \(For example, Skynet Labs runs [siasky.net](https://siasky.net/).\) The **webportal does not host the data!** Instead, it pays hosts in Siacoin on behalf of the user. On Skynet, "pinning" only means monitoring a [file's health](../skynet-topics/health-on-skynet.md) and, if it goes too low, paying additional hosts to bring the file up to full health.

This means that even if [siasky.net](https://siasky.net) were taken down tomorrow or removed its [Free Tier](https://support.siasky.net/siasky.net-accounts/sign-up-and-pick-a-tier#account-tiers), your data availability wouldn't be at risk. Most skyfiles are stored at 10x redundancy, which means 90% of their hosts can drop off the network and your file is still accessible. If any user on the network re-pinned the files before those contacts expired, the data would remain fully available across the network.

### Performance

#### Cached Performance

In many real-world scenarios, the performance of IPFS vs Skynet actually has nothing to do with the underlying technologies. If the requested file is cached by the portal/gateway, your speeds and time-to-first-byte will depend on the webserver that has cached the requested file.

#### Uncached Performance

In uncached environments, Skynet is more performant and predictable for accessing random data. Skylinks hold hints for where to find chunks of data on the network and hosts can be directly asked for the data, instead of working through a P2P network to find the data. Hosts get paid for the reliability and speed, and because of this, compete to maintain a solid reputation.

To see uncached performance metrics, see [https://siasky.tools/](https://siasky.tools/). We use this tool internally to keep track of server health and monitor the worst-performing 99.9% of requests.

## What's next

Interested in migrating your app to Skynet? The front-end is an obvious place to start, but check out our [Getting Started](../getting-started.md) article or our [Introduction Workshop](../skynet-workshops/introduction-workshop/) to start learning how you can architecture your app from the ground up to prioritize decentralization, user-owned data, and censorship resistance with Skynet's various data types.

## Further Reading

{% embed url="https://blog.sia.tech/skynet-summer-2021-update-86ed8db21eae" caption="" %}

