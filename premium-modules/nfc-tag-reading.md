# NFC tag reading

### Overview

This module allows the reading of NFC tags on iOS. We provide a javascript native bridge to initiate tag reading while the app is open. On supported devices running iOS 12 or later, we also support background tag reading for apps with universal links.

### Implementation Guide

#### Check for NFC availability

Create a function that will receive the availability information. Then open the url `gonative://nfc/status?callback=CALLBACK`. 

For example:

```javascript
// define callback function, do not call it yourself!
function nfcStatusCallback(data) {
      if (data.available) {
            // show user NFC-related things, e.g. a button to start scanning
      }
}

// initiate scan
window.location.href = 'gonative://nfc/status?callback=nfcStatusCallback';
```

#### Initiate tag reading

Create a function that will receive the tag data. Then open the url gonative://nfc/readTag with query params:

* callback: a function to receive the data
* message: a string that will be shown in the iOS prompt when scanning for tags
* openUrl: if set to “true” and the the NFC tag has an http or https url, automatically open the url. Data will not be sent to the callback function.

The callback function will be called with an object with the following fields:

* error: if it exists, scanning encountered an error. May be “SystemIsBusy”, “SessionTimeout”, “SessionTerminatedUnexpectedly”, or “Error”.
* prefix: the NDEF tag prefix, e.g. “https://”
* content: the rest of the tag payload after the prefix
* uri: the concatenation of the prefix and the content

For example:

```javascript
var message = 'Hold your device near the NFC tag';

window.location.href = 'gonative://nfc/readTag?callbackFunction=readTagCallback&message=' + encodeURIComponent(message);

function readTagCallback(data) {
    if (data && data.error) {
        console.log('There was an error scanning for NFC tags');
        return;
    }
    if (data && data.uri) {
        // do something with data.uri
    }
}
```

To simply open any http\(s\) url the tag contains:

```javascript
var message = 'Hold your device near the NFC tag';
window.location.href = 'gonative://nfc/readTag?openUrl=true&message=' + encodeURIComponent(message);
```

