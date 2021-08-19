# Getting Started

## Uploading a Site to Skynet

To get started deploying a site on Skynet, create a folder with an `index.html` file in it and all css and javascript linked with relative paths. Visit [http://siasky.net/](http://siasky.net/), select "Do you want to upload an entire directory?" then upload the directory.

![Uploading a site to Skynet using the siasky.net uploader](.gitbook/assets/image%20%2812%29.png)

This works for static sites, along with client-side rendered Single Page Applications. Check out our [Skynet Introduction Workshop](skynet-workshops/introduction-workshop/) for a more in-depth walkthrough.

{% hint style="info" %}
Using `create-react-app`? Add `"homepage": "."` to your `package.json` before you build, then just upload your build folder!
{% endhint %}

## Tools and Resources

Official Documentation and SDKs

* [**Skynet SDK Docs**](https://siasky.net/docs) – Documentation for all the SDKs
* **SDKs** –
  * Browser JS: [skynet-js](https://github.com/NebulousLabs/skynet-js)
  * NodeJS: [@skynetlabs/skynet-nodejs](https://www.npmjs.com/package/@skynetlabs/skynet-nodejs)
  * Python: [python-skynet](https://github.com/SkynetLabs/python-skynet)
  * GoLang: [go-skynet](https://github.com/SkynetLabs/go-skynet)\*
  * CLI: [skynet-cli](https://github.com/SkynetHQ/skynet-cli)\*

{% hint style="warning" %}
SDKs marked with \* above are partial implements – only `skynet-js` implements the registry, SkyDB, MySky, and resolver skylinks. It is our recommended SDK for the time being.
{% endhint %}

### Deployment Tooling

* \*\*\*\*[**Load skynet-js Gist**](https://gist.github.com/m-cat/2d2c0dbaf805658537344c68f3d0f7ef) –Code snippet for loading `skynet-js` in browser, without build tools and without relying on a specific portal.
* [**Deploy to Skynet Github Action**](https://github.com/SkynetLabs/deploy-to-skynet-action) – Github action that deploys a directory to Skynet and comments on pull request with a skylink url. See [Automated deployments on Skynet](https://blog.sia.tech/automated-deployments-on-skynet-28d2f32f6ca1) for using with Handshake.
* [**SkyDeploy**](https://github.com/redsolver/skydeploy) – Command-Line Tool for easily deploying web apps on Skynet and setting the skyns records to point your HNS domain to the new version
* [**Upload2Cloud**](https://github.com/cycleworm/Upload2Cloud) – Windows Explorer integration for sending files and directories to Skynet. Python script available.
* [**Vue CLI Deployment Plugin**](https://github.com/Delivator/vue-cli-plugin-skynet) – Vue UI based tool for site upload with auto Namebase/Handshake updates

### Community SDKs & Initiatives

* [**Skynet SDK for Dart**](https://github.com/redsolver/skynet) – Fully-featured Skynet and MySky SDK for use in Dart and Flutter projects
* [**py-skydb**](https://github.com/PowerLoom/py-skydb) – Python Wrapper that you can use to interact with SkyDB
* \*\*\*\*[**skynet-swift**](https://github.com/ppamorim/skynet-swift) – Swift SDK for uploads, download, registry and SkyDB
* \*\*\*\*[**ruby-skynet**](https://github.com/Beyarz/ruby-skynet) – Ruby SDK for uploads and downloads
* [**Skystandards**](https://github.com/SkynetHQ/skystandards) – A proposal for data standards to be adopted in Skynet applications in such a way that users can share and use their data in different Skynet apps

## Is Skynet the right platform for my website/app?

If your website is structured to be client-side, then hosting it on Skynet is straightforward. If not, it is likely your website will need to reconsider some parts of its architecture to be fully-functional on Skynet.

So, if you use Create React App or Gatsby, deploying to Skynet is as easy as uploading the folder of your production build. If you use a server-side backend like ExpressJS or a CMS like Django or Wordpress, these parts of your website cannot be run on Skynet. You can, however, host your html, css and javascript code on Skynet with your server API elsewhere, but that's not really a decentralized app and isn't what we'd call a skapp.

If you're considering developing on Skynet, take a look at the Skynet Guide's [Skynet Basics](https://support.siasky.net/getting-started/skynet-basics) section to learn some basics or reach out on our [Discord server](https://discord.gg/skynetlabs) in the `#skynet` or `#app-dev` channels.

## Best Practices for Developing on Skynet

Many of these will be incorporated into our Developer Guides, but in the meantime, some things to keep in mind:

* For skapps, it's best to use and link URLs that are a subdomain of the portal domain. That means either using an HNS name as a subdomain like `<skapp>.hns.siasky.net` or a [Base32-encoded subdomain](https://support.siasky.net/key-concepts/skylinks#base32-subdomains). 
* If linking to code host on Skynet that is external from your project, you should use immutable skylinks when possible so that later updates don't risk breaking your code.
* To be a proper "skapp," your application shouldn't rely on external, centralized services to function. This means external requests to APIs for on-network storage or computation that would make your skapp's data not interoperable with other skapps. Web3 apps that use wallets to interact with blockchains are a good example of relying on external, decentralized services.
* Do not hardcode a specific portal domain into your code! This can be helpful for local development, but be sure to remove this before deploying to Skynet. Otherwise, the code is forced to use a portal other than the one it is being served from and works against the decentralized design of Skynet.

