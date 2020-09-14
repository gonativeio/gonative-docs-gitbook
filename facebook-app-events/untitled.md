# Overview

Facebook App Events enable you to view analytics, measure Facebook ad performance and build audiences for Facebook ad targeting using the Facebook SDK.

If enabled, and properly configured, your GoNative app will automatically integrate the Facebook SDK within your app, and enable the app to use Facebook's App Events.

You will need to add your Facebook Application Id and Facebook Display Name to your GoNative app's configuration. You will also need to enter the hash of your GoNative app's "release key" in your Facebook app settings on the Facebook side of things. You will find this hash value on your GoNative app's edit page under Facebook App Events.

GoNative is currently using Facebook SDK version 5.0.0 and supports additional APIs to send custom events to Facebook.

To send an event to Facebook, create a javascript object with the fields:

* event: the string that identifies the event type
* parameters \(optional\): an object of additional parameters
* valueToSum \(optional\): a number to sum up

Convert to JSON, url encode the JSON, and append it to the url `gonative://facebook/events/send?data=URLENCODEDJSON`. For example:

```javascript
var data = {
	event: 'fb_mobile_level_achieved',
	parameters: {
		'fb_level': '1'
	}
};
window.location.href = 'gonative://facebook/events/send?data='
 + encodeURIComponent(JSON.stringify(data));
```

There is a special case for purchases, where Facebook's SDK will send the event more immediately:

```javascript
var data = {
	purchaseAmount: 2.56,
	currency: 'USD'
};
window.location.href = 'gonative://facebook/events/sendPurchase?data='
 + encodeURIComponent(JSON.stringify(data));
```

The list of string constants for event names and parameter keys can be found here:

* [https://github.com/facebook/facebook-objc-sdk/blob/master/FBSDKCoreKit/FBSDKCoreKit/FBSDKAppEvents.m](https://github.com/facebook/facebook-objc-sdk/blob/master/FBSDKCoreKit/FBSDKCoreKit/FBSDKAppEvents.m)
* [https://github.com/facebook/facebook-android-sdk/blob/master/facebook-core/src/main/java/com/facebook/appevents/AppEventsConstants.java](https://github.com/facebook/facebook-android-sdk/blob/master/facebook-core/src/main/java/com/facebook/appevents/AppEventsConstants.java)

