# Offline download manager

  
_Once the premium module has been added to your app, you may use the following APIs to access its functionality._

### Initialization

Create a javascript function to receive status events, e.g.

```javascript
function downloadCallback(data) {
  console.log(data);
}

```

Before starting any downloads, you will need to register your callback by opening this url:

`gonative://downloads/init?callback=downloadCallback`

You may open this url via javascript, using:

`window.location.href = ‘gonative://downloads/init?callback=downloadCallback’;`

### Starting downloads

To start a download, open the url:

`gonative://downloads/downloadFile?url=URL&title=Title`

All query params should be url encoded. "url" and "title" are required, all others are optional.

Supported query parameters are:

* url: The url of the file to download. Should start with http or https.
* title: The name to show in the UI
* identifier \(optional\): a string used to identify the download in the downloadCallback function, so that multiple simultaneous downloads can be differentiated
* details \(optional\): additional information to show below the title. A description of the download.
* date \(optional\): a date in yyyy-mm-dd format to show below the details. Can be used to show a publish date for a podcast, for example.

After a download is started, the callback function will be called with a data object with the following fields:

* identifier: the identifier passed to the downloadFile command
* event: "progress", "done", or "error"

If event is "progress":

* bytesWritten: the number of bytes that have been downloaded
* expectedBytes: the file sized indicated by the server

If event is "error":

* errorMessage: A string indicating the reason for the error

### Showing UI

To show the download manager user interface, open the following url:

`gonative://downloads/showUI`

