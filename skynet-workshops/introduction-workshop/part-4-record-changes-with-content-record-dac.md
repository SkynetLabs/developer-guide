# Part 4: From Shared Data to Shared Logic with DACs

## The Context

In part 3, we saw how to save mutable files on Skynet using MySky. In this section, we'll see how to load that data from MySky, and how to use the Content Record DAC to tell others you have made new content or interacted with existing content.

{% hint style="success" %}
DACs (Data Access Controllers) are like plugins for MySky. Each one exposes an API so that web apps can share code and resources. This is especially useful when MySky data will get used by several applications. DACs let you not worry about messing up a data structure and instead focus on building your application.
{% endhint %}

{% hint style="warning" %}
The Content Record DAC rarely gets used outside of this tutorial. For more powerful and useful DACs, see [Commonly Used DACs](../../skynet-topics/mysky-and-dacs/data-access-controllers.md#commonly-used-dacs).
{% endhint %}

## The Code

DACs provider Javascript libraries that simplify interacting with the web app from your code.

1\. Install `content-record-library` by running `yarn add @skynetlabs/content-record-library`

2\. Next we need to import the DAC. Look for where _Step 4.2_ code goes, and paste the following code.

```javascript
import { ContentRecordDAC } from '@skynetlabs/content-record-library';
```

3\. Now, we'll create a `contentRecord` object, used to call methods against the Content Record DAC's API. For _Step 4.3_, paste the following code.

```javascript
const contentRecord = new ContentRecordDAC();
```

4\. We need to tell MySky to load our DAC. This also informs it of the permissions our DAC will need for a successful login. Return to the code from _Step 3.2_ and uncomment the following code.

```javascript
await mySky.loadDacs(contentRecord);
```

5\. Let's wire up our "Load Data" button, by grabbing data from MySky in the `loadData` method. Add the following code for _Step 4.5_.

```javascript
// Use getJSON to load the user's information from SkyDB
const { data } = await mySky.getJSON(filePath);

// To use this elsewhere in our React app, save the data to the state.
if (data) {
  setName(data.name);
  setFileSkylink(data.skylinkUrl);
  setWebPageSkylink(data.dirSkylink);
  setWebPageSkylinkUrl(data.dirSkylinkUrl);
  setUserColor(data.color);
  console.log('User data loaded from SkyDB!');
} else {
  console.error('There was a problem with getJSON');
}
```

6\. Here, we'll add logic for saving edited content to MySky without re-uploading our image and directory. Then, we'll call the Content Record DAC and have it record that we "interacted" with this skylink. This will occur when the "Save Data and Record Update Action" button is pressed, triggering the `handleSaveAndRecord` method. Add the following code at _Step 4.6_.

```javascript
console.log('Saving user data to MySky');

const jsonData = {
  name,
  skylinkUrl: fileSkylink,
  dirSkylink: webPageSkylink,
  dirSkylinkUrl: webPageSkylinkUrl,
  color: userColor,
};

try {
  // write data with MySky
  await mySky.setJSON(filePath, jsonData);

  // Tell contentRecord we updated the color
  await contentRecord.recordInteraction({
    skylink: webPageSkylink,
    metadata: { action: 'updatedColorOf' },
  });
} catch (error) {
  console.log(`error with setJSON: ${error.message}`);
}
```

7\. Returning to our logic from before, we want to tell the Content Record DAC that we've created a new certificate when we upload a webpage. Add the following code for _Step 4.7_.

```javascript
try {
  await contentRecord.recordNewContent({
    skylink: jsonData.dirSkylink,
  });
} catch (error) {
  console.log(`error with CR DAC: ${error.message}`);
}
```

**8. Test it out!** Now the user can update the color of the halo which is read from our MySky data! You can also use the [Content Record Viewer](http://skey.hns.siasky.net) tool to see your content record.
