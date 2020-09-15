# iOS Passbook

_Once the premium module has been added to your app, you may use the following APIs to access its functionality._

### Overview 

Client web server is responsible for creating pass packages for download onto user’s devices. A pass package consists of a JSON file that defines several data elements, including any bar codes, images, and optionally localization files \(language-specific images and strings\). Each pass has a type, which needs to be registered with Apple like bundle ids are, e.g. pass.com.example.event-ticket. Each pass needs to have a unique serial number, which is an opaque string used to identify the instance of a pass. To update an existing pass, a new pass needs to be created with the same serial number. Passes are automatically synced between a user's devices, and to their Apple Watch.

### Implementation 

The package needs to be signed by a key associated with an Apple developer account, and then zipped up. The resulting package can be distributed via email, a web URL, or downloaded directly within the app.

In the app, the user may click on a link to download a pass. The app will detect that a URL is going to download a pass package. Then the app will download the file itself through a separate download process, and pass the resulting data to the OS. Apple recommends the app check the URL’s mime type to determine whether it is a pass package, therefore the web server should specify the mime type as "application/vnd.apple.pkpass".

For tickets, Client may also specify "relevantDate" and "location" fields in the pass package. Then, when the time to an event approaches, and/or the user is near the venue, the pass will automatically show up on the lock screen.

### Linking from pass back to app 

The app will support an integration point to link the pass back to the app, so the user can go from a pass in Wallet to an event or ticket page in the app. The JSON file in the pass package needs to specify the app's App Store identifier, as well as the URL to give to the app. Therefore, when Client creates the pass package, please set the proper "appLaunchURL" and "associatedStoreIdentifiers" fields. The app will then handle the callback and open the specified URL internally.

### Updating existing passes over the air \(OPTIONAL\) 

A complex integration would be to allow Client’s servers to push updates to the passes. The mechanism works as follows:

* JSON in pass file specifies web service URL
* When pass is added to wallet, the device registers with the web service URL. It POSTs a pushToken.
* When pass needs to be updated, server sends a push notification to the device.
* Device queries the web service URL and downloads an updated pass.
* Device can also notify server it should be unregistered. This happens when the pass is removed by the user.
* An authenticationToken should be specified in the pass package JSON file. This is passed back to the web service URL as a header in each request.

### Additional resources

**Web service reference** [https://developer.apple.com/library/content/documentation/PassKit/Reference/PassKit\_WebService/WebService.html\#//apple\_ref/doc/uid/TP40011988](https://developer.apple.com/library/content/documentation/PassKit/Reference/PassKit_WebService/WebService.html#//apple_ref/doc/uid/TP40011988)

**Developer guide** [https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/PassKit\_PG/index.html](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/PassKit_PG/index.html)

**Pass package reference** [https://developer.apple.com/library/content/documentation/UserExperience/Reference/PassKit\_Bundle/Chapters/Introduction.html](https://developer.apple.com/library/content/documentation/UserExperience/Reference/PassKit_Bundle/Chapters/Introduction.html)

