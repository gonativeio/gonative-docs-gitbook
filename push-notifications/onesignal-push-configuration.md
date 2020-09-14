# OneSignal push configuration

### **Setting up your OneSignal Account**

GoNative leverages [OneSignal's](https://onesignal.com) push notification service to power push notifications within your GoNative app. OneSignal offers their feature-rich, enterprise-class service with a very good free tier offering. To get started, please create an account at [OneSignal.com](https://onesignal.com/).

### **Configure OneSignal**

OneSignal will require two items for Android, and one item for iOS.

For Android, OneSignal sends push notifications through Google's Firebase Cloud Messaging \(FCM\) service. For this to work, you will need to tell OneSignal your Google Project Number, and Firebase Server API Key. A complete guide for this is available on OneSignal's website at [https://documentation.onesignal.com/docs/generate-a-google-server-api-key](https://documentation.onesignal.com/docs/generate-a-google-server-api-key).

Once you have a Google developer account set up, you may generate your Project Number and FCM Server API Key directly at [https://developers.google.com/mobile/add?platform=android&cntapi=gcm](https://developers.google.com/mobile/add?platform=android&cntapi=gcm).

For iOS, OneSignal sends push notifications through Apple's APNS service. For this to work, you will need to upload to OneSignal your Apple Push Notification certificates that you generate through [developer.apple.com](https://developer.apple.com). A complete guide for this is available on OneSignal's website at [https://documentation.onesignal.com/docs/generate-an-ios-push-certificate](https://documentation.onesignal.com/docs/generate-an-ios-push-certificate).

### **Configure your GoNative App**

GoNative requires just your OneSignal App Id in order to configure OneSignal to work within your app. You will find this on your OneSignal Dashboard under App Settings -&gt; Keys & IDs.

### **Sending Notifications with OneSignal**

Whenever a user installs your app and launches it for the first time, it will register itself to receive push notifications and a device record will show up within your OneSignal dashboard. You'll then be able to send push notifications to your users through OneSignal's online dashboard, or programmatically through their Server REST API.

### Opening URL when notification is clicked

OneSignal has a field called Launch URL which will open the specified URL in the user's mobile browser. It is not possible to override that behavior. In order to open a specific URL when the push notification is clicked, leave Launch URL empty, and add a new field in additional data called targetUrl where you may specify the URL to open within the app.

![Specify targetUrl to open URL when notification is clicked](https://gonative.io/images/docs/targetUrl.png)

You may send the same push notification to all users at once, or you may also send personal push notifications, or push notifications to groups of users. Please see [Sending personalized push](sending-personalized-push.md) for more information about associating push tokens with users.
