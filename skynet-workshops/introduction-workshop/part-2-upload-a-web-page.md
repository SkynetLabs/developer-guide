# Part 2: Upload a Web Page

## The Context

In Part 1, our app successfully uploaded a file to Skynet. Now we'll build on that code to upload a web page. Typically you wouldn't write a web app to deploy your web app, but maybe our application is all about letting any user generate and deploy a website.

For this project, our app generates a website's HTML using code stored in `/src/helpers/generateWebPage.js` . This website generated and deployed to Skynet will reference the skylink from Part 1 that points to the user's image. The website generated will display that upload, stored on Skynet, in an `<img>` component.

In Part 2, the web app will upload a directory to Skynet, which must be defined by a JSON object. We'll name the web page's main file `index.html` so that Skynet will serve this file if anyone visits the skylink in a web browser.

## The Code

In addition to files, Skynet can receive directory uploads. Once uploaded to Skynet, any directory with an `index.html` will load in your browser just like any traditional website. This enables developers to write and deploy their web app, just by uploading the project's build folder.

1\. First, create the upload directory functionality. Back in `handleSubmit` inside `src/App.js`, paste this code in the area for _Step 2.1_.

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

// Generate a URL for our current portal
// We'll use a subdomain-style link
const dirSkylinkUrl = await client.getSkylinkUrl(dirSkylink, {
  subdomain: true,
});

console.log('Web Page Uploaded:', dirSkylinkUrl);

// To use this later in our React app, save the URL to the state.
setWebPageSkylink(dirSkylink);
setWebPageSkylinkUrl(dirSkylinkUrl);
```

2\. Above this code, uncomment `console.log('Uploading web page...');`

**3. Test it out!** Now the user can submit their name and photo to generate their very own web page on Skynet!

## The Result

![Our finished webpage, referencing the skylink of our uploaded image.](<../../.gitbook/assets/image (8).png>)
