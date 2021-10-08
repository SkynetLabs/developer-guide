# Additional Skynet Tools

## Tools and Resources

* [**Skynet SDK Docs**](https://siasky.net/docs) – Documentation for all the SDKs
* **SDKs** –
  * Browser JS: [skynet-js](https://github.com/NebulousLabs/skynet-js)
  * NodeJS: [nodejs-skynet](https://github.com/SkynetHQ/nodejs-skynet)\*
  * Python: [python-skynet](https://github.com/SkynetHQ/python-skynet)\*
  * GoLang: [go-skynet](https://github.com/SkynetHQ/go-skynet)\*
  * CLI: [skynet-cli](https://github.com/SkynetHQ/skynet-cli)\*

{% hint style="warning" %}
SDKs marked with \* above are not fully implemented – only skynet-js implements SkyDB and is our recommended SDK for the time being.
{% endhint %}

## Community SDKs & Initiatives

* [**Skynet SDK for Dart**](https://github.com/redsolver/skynet) – Use Sia Skynet, SkyDB and MySky in your Dart and Flutter projects
* [**Skynet-swift**](https://github.com/ppamorim/skynet-swift) – Use Sia Skynet in your iOS or macOS projects using Swift
* [**py-skydb**](https://github.com/PowerLoom/py-skydb) – Python Wrapper that you can use to interact with SkyDB portals
* [**Skystandards**](https://github.com/SkynetHQ/skystandards) – a proposal for data standards to be adopted in Skynet applications in such a way that users can share and use their data in different Skynet apps
* [**Zenbase**](https://github.com/Fluffy9/Zenbase) – Skynet storage adapter for [GunDB](https://gun.eco/)

## Developer and Deployment Tooling

* [**Rift**](https://riftapp.hns.siasky.net/#/) – A site featuring lots of tools to help explore Skynet applications. Includes an uploader, MySky data explorer, resolver skylink manager for DNS, and more.
* [**skynet-cli**](https://github.com/SkynetHQ/skynet-cli) – Great tool for uploading directories from the command line.
* [**SkyDeploy**](https://github.com/redsolver/skydeploy) – Command-Line Tool for easily deploying web apps on Skynet and setting the skyns records to point your HNS domain to the new version
* [**skylinkv2-cli-tool**](https://github.com/peterjan/skylinkv2-cli-tool) – Tool for managing and updating resolver skylinks \(previously "skylink v2"\) from the command-line
* [**Deploy to Skynet Github Action**](https://github.com/kwypchlo/deploy-to-skynet-action) – Github action that deploys a directory to Skynet and comments on pull request with a skylink url. See [Automated deployments on Skynet](https://blog.sia.tech/automated-deployments-on-skynet-28d2f32f6ca1) for using with Handshake.
* [**Upload2Cloud**](https://github.com/cycleworm/Upload2Cloud) – Windows Explorer integration for sending files and directories to Skynet. Python script available.
* [**Vue CLI Deployment Plugin**](https://github.com/Delivator/vue-cli-plugin-skynet) – Vue UI based tool for site upload with auto Namebase/Handshake updates
* [**Load skynet-js Gist**](https://gist.github.com/m-cat/2d2c0dbaf805658537344c68f3d0f7ef) –Code snippet for loading `skynet-js` in browser, without build tools and without relying on a specific portal.

## Example Code Repos

* [**Hackathon Leaderboard**](https://github.com/SkynetLabs/leaderboard-website) – Mixes an API exposing the content record scraper with direct MySky access.
* [**Content Record Scraper**](https://github.com/SkynetLabs/leaderboard-website) – Scraper for Discoverable Files using the Content Record and Feed DAC using known User IDs.
* [**MongoDB Restore with Skynet + Akash**](https://github.com/peterjan/akash-mongo-restore) – Example Akash deployment using Skynet to automatically backup and restore MongoDB.
* [**MySky + React Template**](https://github.com/dghelm/skynet-react-template) – Sample Template for using MySky in a Create React App, using React [Context](https://github.com/dghelm/skynet-react-template/blob/main/src/state/SkynetContext.js) and Easy Peasy to manage application state.

