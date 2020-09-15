# Biometric Authentication \(TouchId / FaceId\)

## Overview

Fingerprint technology has been accessible to apps running on iOS devices starting with the iPhone 5s running iOS 9.0, and Android devices starting with Android 6.0 \(Marshmallow\). GoNative's fingerprint module enables this convenient and secure means of authenticating users of your app. FaceID is also supported for newer iOS devices.

Our TouchID / Fingerprint module uses a javascript-based API to secure storage and retrieval of user credentials. Communication from the web page to the native apps is done by opening a specific gonative://auth url with certain query parameters, detailed below.

Responses from the native app are delivered to a callback function in the window's javascript context. The name of the function is specified when the gonative://auth url is opened.

The API allows the storage of a secret string. The contents and format of the string should be decided by the website developer. It may contain a JSON object containing the username and password, an authentication token, or anything else that would authenticate the user.

The first step is to check for the presence and availability of the fingerprint reader. If there is no fingerprint reader, or if the user has not enrolled any fingerprints, a fingerprint dialog should not be shown. If fingerprints are available, then the status check will also indicate the presence of a previously saved secret.

Once a user has logged in, and has fingerprints enrolled, the website may save the secret. We use the iOS Keychain and Android Keystore technologies, which in turn leverage cryptographic hardware, to ensure that the secret cannot be retrieved without fingerprint authentication from the user. Note that saving the secret does not require any specific user interaction on the device.

The next time the user needs to log in to the website, and the status check indicates a secret is available, the website should attempt to retrieve the secret from the device. At that point, the user will be shown a dialog prompting them to authenticate via their fingerprint. If the fingerprint check is successful, the secret will be sent to the website via the javascript callback. If not, the callback will receive an error.

## Implementation Guide

This guide assumes a working publicly-accessible website \(or test site\) with a username and password login system.

### Save secret on successful login

After a user successfully logs in to your website with their username and password, check to see if TouchID is available. If it is, save a secret for future retrieval. The secret must be a single string and can be a combination of the username and password, or an authentication token.

For example, you may embed this javascript into your post-login page:

```javascript
var username = 'andy'
var password = 'password';

window.location.href = 'gonative://auth/status?callbackFunction=gonative_status_afterlogin';

function gonative_status_afterlogin(data) {
    if (data && data.hasTouchId) {
    	var secret = JSON.stringify({
            username: username,
            password: password
        });
        
        window.location.href = 'gonative://auth/save?secret=' + encodeURIComponent(secret);
    }
}

```

In this example, we have saved the username and password as the secret. You may choose to save an authentication token instead.

### Check for secret on login page

On the login page, you will need to know whether or not to prompt for TouchID credentials. Start by getting the status:

```javascript
window.location.href = 'gonative://auth/status?callbackFunction=gonative_status_beforelogin';

function gonative_status_beforelogin(data) {
   if (data && data.hasTouchId && data.hasSecret) {
       // Prompt the user to use the fingerprint to log in
       window.location.href = 'gonative://auth/get?callbackFunction=gonative_secret_callback';
   }
}

function gonative_secret_callback(data) {
    if (data && data.success && data.secret) {
        var credentials = JSON.parse(data.secret);
        var username = credentials.username;
        var password = credentials.password;
        
        // Use username and password to do login here,
        // e.g. an http post or ajax request
    } else {
        // Allow manual entry
    }
}

```

Once the gonative\_secret\_callback function is called with the previously saved secret, it should perform a request to log in the user. If the credentials are incorrect, you should delete the secret and allow manual login.

```javascript
// delete secret if credentials are incorrect
window.location.href = 'gonative://auth/delete';

```

## API Reference

### Retrieving TouchID availability

Open the url `gonative://auth/status?callbackFunction=CALLBACK`

Callback is required. The app will execute CALLBACK with an object parameter containing the fields:

- hasTouchId: true or false. Indicates if the device is running iOS 9+ and there are fingerprints enrolled, or FaceID is enabled. Also set to true on Android devices with fingerprints enrolled.

- biometryType: ‘touchId’, ‘faceId’, or ‘none’. This field is populated on iOS only to differentiate between TouchID and FaceID.

- hasAndroidFingerprint: true or false. Indicates that the Android device has fingerprints enrolled.

- hasSecret: true or false

### Saving a secret

Open the url `gonative://auth/save?secret=SECRET&callbackFunction=CALLBACK`

Callback is optional. The app will execute CALLBACK with an object parameter containing the fields:

- success: true or false

- error: provided if success is false \(see error codes below\)

### Retrieving a secret

Open the url `gonative://auth/get?callbackFunction=CALLBACK&prompt=PROMPT`

Callback is required. Prompt is optional, and provides a message that can be displayed when iOS requests the user touch the fingerprint sensor. The app will execute CALLBACK with an object parameter containing the fields:

- success: true or false

- error: provided success is false \(see error codes below\)

- secret: the previously stored secret

Another optional query parameter is callbackOnCancel. If set to 1 and the user cancels the authentication, the callback will be run with success=false, error=userCanceled. If callbackOnCancel is not set \(or set to 0\), the callback will not be run.

### Deleting a secret

Open the url `gonative://auth/delete?callbackFunction=CALLBACK`

Callback is optional. The app will execute CALLBACK with an object parameter containing the fields:

- success: true or false

- error: provided if success is false

### Possible error values

In general, you will only need to handle authenticationFailed in the "get secret" request.

duplicateItem

itemNotFound

authenticationFailed

genericError

userCanceled

unimplemented

### White-listing access

By default, any page loaded in your app will be able to use this javascript API to retrieve secrets. If you are allowing any domains you do not control to be loaded within your app, we strongly recommend whitelisting only certain domains to our Native JS Bridge.

To do so, please add an additional field to your appConfig.json, editable through our website in the Import/Export section when configuring your app.

```javascript
"general": {
  "nativeBridgeUrls": "https?://([-\\w]+\\.)*example\\.com.*"
}

```

nativeBridgeUrls should be either a string or array of strings, in which case url can match any of the regexes. Note that it is double-escaped to be valid JSON syntax.

