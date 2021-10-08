# Adding Homescreen Support to an App

## Introduction

Preparing your app for deployment on Homescreen should not require many changes if the app is already designed for decentralized usage. 

{% embed url="https://www.youtube.com/watch?v=rK8diBGeXeg&t=78s" caption="Video Tutorial for Adding Homescreen to an App. Demo starts at 11:15." %}

## Supporting Homescreen in your application

### 1. Confirm your site works when deployed to Skynet

Build your web app locally and upload it as a directory to Skynet. See the [final part](../../skynet-workshops/introduction-workshop/part-5-deploy-the-web-app-on-skynet.md) of the Introduction Workshop for more info. Confirm behavior and links work as they should. _\(See video above at 14:25\)_

{% hint style="info" %}
We recommend avoiding usage of centralized APIs or other resources that lack long-term support for users needing to access back-end data with older versions of a front-end.
{% endhint %}

### 2. Setup a resolver skylink that will point at your latest build

Our [Deploy-to-Skynet Github Action](../../developer-guides/deploy-github-actions.md) is the easiest way to automate updating your resolver skylink on every commit to your Github repo. _\(See video above at 18:47.\)_ You'll want to update your resolver skylink on each new deployment, and [Resolver Skylinks](../../skynet-topics/resolver-skylinks.md#web-tools) shows more tooling for creating and updating resolver skylinks.

### 3. Configure your Manifest file

Many web applications create a `Manifest` file by default and link it in the `<HEAD>` of the app's `index.html` file. Homescreen first looks for that manifest file to learn about your app. For the best user experience, be sure to set the following fields:

* `short_name` or `name`
* `description`
* an object in the `icons` array
* `theme_color`
* `skylink`

By setting the resolver skylink for your application here, Homescreen will be able to check for updates to the application. Read more about webmanifest files at web.dev's [Add a web app manifest](https://web.dev/add-manifest/) article. Here is [an example manifest](https://siasky.net/AQBTlVUdVT_qLqqA_4umNe8aiO6KxoGbfvWzEEk0OyvF7w/manifest.json) from our Uniswap fork.

### 4. Add the Homescreen button on your project's Readme

You'll want to add the following code in your project's Readme, replacing `[skylink]`with your resolver skylink \(excluding `sia://`\). _\(See video above at 27:40.\)_

```text
[![Add to Homescreen](https://img.shields.io/badge/Skynet-Add%20To%20Homescreen-00c65e?logo=skynet&labelColor=0d0d0d)](https://homescreen.hns.siasky.net/#/skylink/[skylink])
```

{% hint style="info" %}
You can include this for specific "Releases" as well. If you do, avoid using the resolver skylink, and instead use the skylink of the specific deployment for the release version.

Then, users can install older versions of front-end but always be able to update, since the resolver skylink in the app's manifest file will point to your latest release.
{% endhint %}

### 5. Confirm it works!

Commit the changes, try the link, and if you're logged in to MySky, you should see a prompt asking you to "Add to Homescreen". That's it! Your app is ready to be used by Homescreen users.

![Save to Homescreen pop-up. See extended detalis to confirm Manifest info.](../../.gitbook/assets/image%20%2814%29.png)



## Additional Resources

{% embed url="https://www.youtube.com/watch?v=ZbGb7FppaZ4" caption="Video Introduction for Integrating Homescreen, including with an HNS name" %}

