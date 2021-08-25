---
description: MySky or mention React Context as a way to use only one SkynetClient?
---

# Skynet with React

In most instances, React and React-based frameworks work seamlessly with Skynet. Here are some tips to keep in mind when using [Create React App](https://github.com/facebook/create-react-app).

{% hint style="info" %}
We use Create React App in the Introduction Workshop, so you can refer to that codebase as well.
{% endhint %}

## Use HashRouter when using React-Router

If you're using [React Router](https://reactrouter.com/) for your page routing, you'll want to use its `HashRouter` instead of `BrowserRouter` .  In many code examples, this replacement happens in the import statement.

```javascript
import {
  // BrowserRouter as Router,
  HashRouter as Router,
  Switch,
  Route,
  Link
} from "react-router-dom";
```

This will modify links to contain `#` before the route. 

{% hint style="info" %}
We are plan to release full support for BrowserRouter and other routing techniques in July/August 2021.
{% endhint %}

## Add "Homepage" to package.json

In order for create-react-app to know how to load your js files, you need to add the following to your application's root `package.json`

```javascript
{
    // ...
    "homepage": ".",
    // ...
}
```

## Build & Deploy

Run your normal build command, then upload the `build` folder containing an `index.html` file to Skynet. When using the Siasky.net homepage, be sure to select the folder tab.

![Select the &quot;Do you want to upload a web app or directory?&quot; Tab before Uploading](../.gitbook/assets/image%20%281%29.png)

## Going Further

Next, consider hosting your website using a [Handshake Name](../integrations/hns-names.md) or a traditional domain name using DNSLink.

Or, consider improving your build pipeline using our [Github Action](deploy-github-actions.md).



