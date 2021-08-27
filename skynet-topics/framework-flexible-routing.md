---
description: >-
  Setup Skynet Deployments to accept various page serving and routing
  configurations to meet the needs of a variety of web frameworks.
---

# Framework-Flexible Routing

Different web frameworks expect different file serving behaviors, especially for routes pointing to dynamic content. Skynet supports advanced configuration for setting the server behavior of an upload. It does this by allowing uploads to attach metadata fields inspired by NGINX – `tryFiles` and `errorPages` .

{% hint style="info" %}
If this seems odd to you, consider that a site \(or skyfile\) doen't have a file available for every `/user/[USERID]` combination. If a user requests that URL though, we want the browser to return a file that can handle the logic for the web application, even though that file doesn't exist in the directory uploaded to Skynet.
{% endhint %}

{% hint style="danger" %}
This feature is not yet released and is under active testing and development.

**Please report back any problems or framework-specific results!**
{% endhint %}

## Framework Support

The following frameworks have compatibility on Skynet.

| Framework | Compatibility | Notes |
| :--- | :---: | :---: |
| [React Router](https://reactrouter.com/) | ✅ |  |
| [GatsbyJS](https://www.gatsbyjs.com/) | ✅ | [🔗](framework-flexible-routing.md#gatsby) |
| Traditional Web | ✅ |  |
| Untested Framework | ❔ |  |
| Bad Framework | ❌ |  |

{% hint style="info" %}
Have you tested a framework not included here or think we made a mistake? Reach out on [Discord](https://discord.gg/skynetlabs) or raise an issue on this document's Github repo.
{% endhint %}

{% hint style="warning" %}
Although you are able to define various `errorpages`, a Skynet Webportal will only ever encounter a 404 error on your deployment. Any other errors would result in not accessing your deployment, and thus not being able to serve your error page.
{% endhint %}

## Setting Routing Configuration for Uploads in skynet-nodejs

Right now, we only have upload support in `@skynetlabs/skynet-nodejs 2.2.0`

The below code sample shows some default values and using them with your Skynet client.

```javascript
const skynetNode = require('@skynetlabs/skynet-nodejs');

// Feature is only available on dev server for now.
const server = 'https://siasky.dev';

// Initialize client
let nodeClient = new skynetNode.SkynetClient(server);

// Sensible defaults for some project types
const routingPresets = {
  gatsby: {
    errorPages: { 404: '/404.html' },
    tryFiles: ['index.html', '/index.html'],
  },
  gatsbyStatic: {
    errorPages: { 404: '/404.html' },
    tryFiles: ['index.html'],
  },
  reactRouter: {
    tryFiles: ['index.html', '/index.html'],
  },
  traditional: {
    errorPages: { 404: '/404.html' },
    tryFiles: ['index.html'],
  },
};

// Select presets
const {tryFiles, errorPages} = routingPresets.reactRouter;

// Pass routing configuration into uploadDirectoy's
// custom objects.
const response = await nodeClient.uploadDirectory(path, {
      tryFiles,
      errorPages,
});
```

{% hint style="info" %}
Wanting to directly call the API? See documentation [here](https://gitlab.com/SkynetLabs/skyd/-/blob/master/doc/api/index.html.md#skynetskyfilesiapath-post). Remember to use `siasky.dev/skynet/skyfile` and to URL-encode the JSON of the parameters.
{% endhint %}

## Testing Repo

We have a repo available for running tests for uploading your project. Please refer to the Readme to learn more.

{% embed url="https://github.com/SkynetLabs/custom-routing-tests" %}

## Choosing Values

When uploading a folder there are two metadata values you're able to set.

1. `tryfiles` – If the file you requested does not exist, the web portal will iterate through the list, trying to return the specified file for the route requested. All directory uploads will now default to `["index.html"]` so for a request to `/foo/` the webportal will serve `/foo/index.html`. Absolute path file names will be returned if no other files are found, so using `["index.html", "/index.html"]` will return your root-level `index.html` file if no other routes match the request. _\(If you have an absolute path defined, this file must exist in your directory, and will be returned with a status 200 when a 404 would otherwise be returned.\)_
2. `errorpages` – The maps a specific HTTP error status code with the absolute-path file to return if the error state is reached. This is not set by default, but for "traditional" style website, we suggest `{ 404: '/404.html' }`.

{% hint style="warning" %}
The API endpoint capitalization differs from the SDK libraries, in order to meet language standards. `skynet-nodejs` and `skynet-js` use camelcase \(`tryFiles`\) while the API endpoint uses lowercase \(`tryfiles`\).
{% endhint %}

## Framework Specific Notes

### Gatsby

We have 2 presets declared for Gatsby above. Some sites are built to be fully static, so every available route is generated at build time. For these projects, you can use `gatsbyStatic` settings and still enable a 404 page.

If your Gatsby site has some routes that are not generated at build time \(`/users/xyz_dynamic/`\), use the `gatsby` configuration and define a 404 behavior in the routes of your application. A 200 status response will be returned for these files.

