# Part 1: Upload a File

## The Context

Assuming you've already [uploaded a file Skynet before](https://support.siasky.net/getting-started/skynet-basics#upload-a-file), here we'll build that upload functionality into our app. We'll get back a skylink and create a link to the file in our webapp.

## The Code

We'll first cover the most basic functionality of Skynet, uploading data.

Follow the steps below to update this app to allow the user to upload a file to Skynet. For this sample app, we'll ask the user to upload a picture.

1. Install `skynet-js` by running `yarn add skynet-js@beta`
2. First, you need to import the SDK and initialize a Skynet Client. Open the file `src/App.js`, look for where _Step 1.2_ code goes, and paste the following code.

```javascript
// Import the SkynetClient and a helper
import { SkynetClient } from 'skynet-js';

// We'll define a portal to allow for developing on localhost.
// When hosted on a skynet portal, SkynetClient doesn't need any arguments.
const portal =
  window.location.hostname === 'localhost' ? 'https://siasky.net' : undefined;

// Initiate the SkynetClient
const client = new SkynetClient(portal);
```

{% hint style="info" %}
In a deployed application, we don't want to "hard-code" the Skynet portal. Instead, we want the browser to use the same portal that the site is being accessed through. So here, if our browser is pointed to "localhost," we use the siasky.net portal, otherwise, we don't define a portal and let the SkynetClient worry about it.
{% endhint %}

1. Next, create the upload functionality. In the `handleSubmit` function \(called for when form is submitted\), paste the code that will upload a file below the _Step 1.3_ mark.

```javascript
// Upload user's file and get backs descriptor for our Skyfile
const { skylink } = await client.uploadFile(file);

// skylinks start with `sia://` and don't specify a portal URL
// we can generate URLs for our current portal though.
const skylinkUrl = await client.getSkylinkUrl(skylink);

console.log('File Uploaded:', skylinkUrl);

// To use this later in our React app, save the URL to the state.
setFileSkylink(skylinkUrl);
```

1. Above this code, uncomment `console.log('Uploading file...');`

**5. Test it out!** If you aren't still running the app, run `yarn start` again and try uploading a file. If you open your Developer Console \(by pressing F12\), the console show helpful messages.

