# Part 2: Upload a Web Page

## The Context

In Part 1, our app successfully uploaded a file to Skynet. Now we'll build on that code to upload a web page.

Our webpage's HTML will be generated using the code in `/src/helpers/generateWebPage.js` . Any This webpage will reference the skylink from Part 1 to display our upload in an `<img>` component.

The upload will be a directory defined by a JSON object. We'll title the webpage `index.html` so that Skynet will serve the webpage if anyone visits the skylink.

## The Code

In addition to files, Skynet can receive directory uploads. Once uploaded to Skynet, any directory with an `index.html` will load in your browser just like any website. This enables developers to write and deploy their web app, just by uploading the project's build folder.

1.First, create the upload directory functionality. Back in `handleSubmit` inside `src/App.js`, paste this code in the area for _Step 2.1_.

```javascript
// Create the text of an html file what will be uploaded to Skynet
// We'll use the skylink from Part 1 in the file to load our Skynet-hosted image.
const webPage = generateWebPage(name, skylinkUrl);

// Build our directory object, we're just including the file for our webpage.
const webDirectory = {
  'index.html': webPage,
  // 'couldList.jpg': moreFiles,
};

// Upload user's webpage
const { skylink: dirSkylink } = await client.uploadDirectory(
  webDirectory,
  'certificate'
);

// generate a URL for our current portal
const dirSkylinkUrl = client.getSkylinkUrl(dirSkylink);

console.log('Web Page Uploaded:', dirSkylinkUrl);

// To use this later in our React app, save the URL to the state.
setWebPageSkylink(dirSkylinkUrl);
```

1. Above this code, uncomment `console.log('Uploading web page...');`

**3. Test it out!** Now the user can submit their name and photo to generate their very own web page on Skynet!

## The Result

![Our finished webpage, referencing the skylink of our uploaded image.](../../.gitbook/assets/image%20%288%29.png)

