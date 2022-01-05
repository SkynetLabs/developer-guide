# Operating a Skynet Webportal

## Introduction

Skynet's decentralization depends not just on architecture, but also on access. Skynet needs a decentralized network of portal operators, and all of our tools for running and maintaining a multi-server web portal are open-source.

If you're unfamiliar with Skynet portals, please see [Web Portals on Skynet](https://support.siasky.net/getting-started/web-portals-on-skynet) in the Getting Started guide and the [Portals](https://support.siasky.net/key-concepts/skynet-portals) support article.

{% hint style="warning" %}
It is very uncommon that a project would _need_ to run a webportal. If you're building a web app or any project where you don't want to run a webportal, you probably don't need to.
{% endhint %}

## Web Portal Components

Running a web portal consists of 3 major services

* ****[**skyd**](https://gitlab.com/SkynetLabs/skyd) – the Skynet daemon used for interacting with the Sia and Skynet networks
* ****[**skynet-webportal**](https://github.com/SkynetLabs/skynet-webportal) – the webportal stack used for exposing skyd to the web
* ****[**skynet-accounts**](https://github.com/SkynetLabs/skynet-accounts) – the service used for creating and maintaining user portal accounts

## Getting Started

{% hint style="info" %}
As of 08/25/2021, we are within weeks of releasing tooling and documentation for making the process of starting and running a portal much easier.
{% endhint %}

Please see the documentation in the [Webportal Management](../webportal-management/requirements.md) section to get started.
