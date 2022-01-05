# MySky & DACs

## Introduction

MySky allows users to have a single login for the entire Skynet ecosystem and take their data with them from application to application. For developers, it enables simple sign-in features, interoperable data between applications, and the ability to read and write application data to Skynet using the credentials of the user but without ever requiring the user to trust your application with their private seed.

With Data Access Controllers (DACs), developers using MySky can leverage powerful decentralized tooling and APIs with the ease of importing an npm module. DACs are used across the Skynet ecosystem for interoperable user profiles, content feeds, and social graphs.

When you upload a file to Skynet, the hash is not connected to the uploader. There are no mechanisms for tracking or remembering what files you uploaded. With MySky Files, users and developers can take advantage of a virtual file system, using paths and access controls that create a flexible way to store, access, and share data.

## Adding MySky to an App

The best place to get started understanding MySky integration for an app is in the Introduction Workshop's [Part 3: Make it Dynamic with MySky](../../skynet-workshops/introduction-workshop/part-3-make-it-dynamic-with-mysky.md). You will see how to add load MySky in your application, add a [dataDomain](mysky-files.md#mysky-permissions-and-datadomains), check to see if a user is logged in, and attach a "login" function to a button to launch the login and permissions window for the users.

## Key Ideas

As David wrote in his announcement of the technology, "[MySky: Your Home on the Global Operating System of the Future](https://blog.sia.tech/mysky-your-home-on-the-global-operating-system-of-the-future-5a288f89825c)":

> In the simplest sense, MySky is a decentralized identity protocol that gives users their own empire of data within Skynet. Everything that happens inside of that empire is under the control of the user, but it can also be visited by anyone else and shared around freely.

## skynet-js 4.0@beta with MySky and DACs

With the latest major beta release of `skynet-js`we've introduced MySky and DACs. Here are some resources for getting started.

* [**Skynet SDK Docs v4 Update Guide**](https://siasky.net/docs/v4/#updating-from-v3) – Documents breaking changes from v3 and outlines the update
* ****[**Skynet Workshop**](https://github.com/SkynetLabs/skynet-workshop) – The latest version of the workshop includes MySky and DAC sections showing the basic functionality of each. _Also see -_ [video recording](https://www.youtube.com/watch?v=TDiLdHQidBE), [deployed project](http://snew.hns.siasky.net) and [companion app](https://my-sky.hns.siasky.net).
* ****[**Content Record Viewer**](https://skey.hns.siasky.net) – Temporary home for sample app that reviews Content Record data.
* ****[**Content Record Library Repo**](https://github.com/SkynetHQ/content-record-library) – When using a DAC, you import their library in your code to make calling the API of the DAC iframe simpler. This repo documents the Content Record DAC methods.

## Architecture

MySky takes advantage of how web browsers sandbox iframes. This allows MySky to secure and isolate a user's seed while still allowing them to sign messages for the host web application.

MySky and the parent application communicate over `postMessage` to coordinate the creation of and access to [MySky Files](broken-reference), following the permissions set by the user.

[DACs](data-access-controllers.md) work similarly. The main codebase of the DAC is located in a small web app which is loaded into an invisible iframe in the host application. The host application loads a "DAC Library" which exposes actions to the developer that, when invoked, will coordinate the action with the DAC. The DAC can be seen as similar to a server-side API. but run client-side in the browser, with protected code that can be shared across many applications and (unlike with software libraries) safe from having its code manipulated by the web app developer.

This lets developers with no knowledge of the user's private key or social graphs use really simple abstractions like `socialDac.followUser('abc')` , that, when called in the parent application just seems to work.

Meanwhile, the DAC code in the iframe can worry about permissions, interoperability with other Skynet applications, efficiency, API versioning with backward compatibility, and other concerns.

![MySky and DAC iframe relationship to Host App](<../../.gitbook/assets/image (13).png>)

## Further Reading

{% embed url="https://blog.sia.tech/mysky-your-home-on-the-global-operating-system-of-the-future-5a288f89825c" %}

{% embed url="https://blog.sia.tech/a-technical-breakdown-of-mysky-seeds-ba9964505978" %}
