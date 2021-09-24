# Part 3: Make it Dynamic with MySky

## The Context

In parts 1 and 2, you uploaded files onto Skynet. The files at these Skylinks are immutable, that is, they cannot be modified \(or else their URL would also change\). In this section, we'll use MySky to store editable data on Skynet that we can come back to and update.

Right now, if you hover over your image in the certificate, you get a nice green halo effect. But, we may want to change this later without changing our skylink. We can do this by saving some editable JSON data to Skynet and having our web page read the info directly from Skynet.

The first step is hooking up our app to `MySky`, but you'll need a little theory here.

MySky lets users login across the entirety of the Skynet ecosystem. Once logged in, applications can request read and write access for specific "MySky Files" that have a file path. In this workshop, we'll be working with "Discoverable Files" -- these are files that anyone can see if they know your MySky UserID and the pathname for the file.

With MySky, we can write a JSON object to a path, and then tell someone our UserID and the pathname, and they can find the latest version of the data.

## The Code

Let's setup MySky login and then look at writing data to a Mysky File.

1. First we need set the `dataDomain` we will request to from MySky. Replace the line of code in _Step 3.1_ with this.

```javascript
// choose a data domain for saving files in MySky
const dataDomain = 'localhost';
```

2. Now we need to add the code that will initialize MySky when the app loads. This code will also check to see if a user is already logged in with the appropriate permissions. Add the following code to `src/App.js` for _Step 3.2_.

```javascript
// define async setup function
async function initMySky() {
  try {
    // load invisible iframe and define app's data domain
    // needed for permissions write
    const mySky = await client.loadMySky(dataDomain);

    // load necessary DACs and permissions
    // await mySky.loadDacs(contentRecord);

    // check if user is already logged in with permissions
    const loggedIn = await mySky.checkLogin();

    // set react state for login status and
    // to access mySky in rest of app
    setMySky(mySky);
    setLoggedIn(loggedIn);
    if (loggedIn) {
      setUserID(await mySky.userID());
    }
  } catch (e) {
    console.error(e);
  }
}

// call async setup function
initMySky();
```

3. Next, we need to handle when a user presses the "Login with MySky" button. This will create a pop-up window that will prompt the user to login. In future versions of MySky, it will also ask for additional permissions to be granted if the app doesn't already have them. Add the following code to `src/App.js` for _Step 3.3_.

```javascript
// Try login again, opening pop-up. Returns true if successful
const status = await mySky.requestLoginAccess();

// set react state
setLoggedIn(status);

if (status) {
  setUserID(await mySky.userID());
}
```

4. We also need to handle when a user logs out. This method will tell MySky to log the user out of MySky and then delete any remaining user data from the application state. Add the following code to `src/App.js` for _Step 3.4_.

```javascript
// call logout to globally logout of mysky
await mySky.logout();

//set react state
setLoggedIn(false);
setUserID('');
```

5. Now that we've handled login and logout, we will return to saving data when a user submits the form. Locate and uncomment `console.log('Saving user data to MySky file...');`

6. After uploading the image and webpage, we make a JSON object to save to MySky. Put the following code in the below _Step 3.6_.

```javascript
// create JSON data to write to MySky
const jsonData = {
  name,
  skylinkUrl,
  dirSkylink,
  dirSkylinkUrl,
  color: userColor,
};

// call helper function for MySky Write
await handleMySkyWrite(jsonData);
```

7. We need to define what happens in the helper method above. We'll tell MySky to save our JSON file to our filePath. Put the following code in the below _Step 3.7_.

```javascript
// Use setJSON to save the user's information to MySky file
try {
  console.log('userID', userID);
  console.log('filePath', filePath);
  await mySky.setJSON(filePath, jsonData);
} catch (error) {
  console.log(`error with setJSON: ${error.message}`);
}
```

8. Next, we want the certificate web page to read this data. The code to fetch a MySky file is already in the generated page, but it needs to know our userID and the file path in order to find the JSON file. Find the code from _Step 2.1_ that says

```javascript
const webPage = generateWebPage(name, skylinkUrl);
```

and replace it with

```javascript
const webPage = generateWebPage(name, skylinkUrl, userID, filePath);
```

**9. Test it out!** Now the user can select the color of the halo which is read from our MySky data! In the next step we'll add loading this data and editing it without re-uploading our image and webpage.

