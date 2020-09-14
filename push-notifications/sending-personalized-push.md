# Sending personalized push

### **Overview**

Are you looking to send push notifications to your users on an individual basis?

Do you want to send your users a push notification when certain events happen, such as:

* when member receives a message
* when members receives comment on post

This is fully supported with GoNative, but requires proper configuration. 

It requires:

1. Ability to associate a user account in your system with the user's device and its OneSiginalUserId
2. Sending push notifications programmatically using OneSignal's Server REST API, based on specific events occurring in your backend

For \#1, please see Associating Users with their Devices below.

For \#2, you will be using OneSignal's Server REST API to send push notifications programmatically. When hitting OneSignal's API, you may specify a set of OneSignalUserIds to target with your message. More info at [https://documentation.onesignal.com/reference.](https://documentation.onesignal.com/reference) 

### **Associating Users with their Devices**

In order to send push notifications to specific users, or groups of users, you will need a way to associate users with their devices.

To accomplish this, you may configure our "registration service". Please see sample registration information below.

```javascript
{
    oneSignalUserId: 'xxxxxxx',
    oneSignalPushToken: 'xxxxxx',
    oneSignalSubscribed: true,
    oneSignalRequiresUserPrivacyConsent: false,
    platform: 'ios',
    appId: 'io.gonative.example',
    appVersion:  '1.0.0',
    distribution: 'release',
    hardware: 'armv8',
    installationId: 'xxxx-xxxx-xxxx-xxxx',
    language: 'en',
    model: 'iPhone',
    os: 'iOS',
    osVersion: '10.3',
    timeZone: 'America/New_York'
}
```

#### **Method \#1 - Access the data directly via javascript**

If your website defines a function called `gonative_onesignal_info`, it will get called after every page load as shown below. You will then have the registration information in javascript, and may then POST it to your server via AJAX, or do anything else with it.

```javascript
// you define this function, but do not actually call it! 
// the app itself will call it after every page load of your app
function gonative_onesignal_info(info) {
    console.log(info);
}

// info will look like
{
    oneSignalUserId: 'xxxxxxx',
    oneSignalPushToken: 'xxxxxx',
    oneSignalSubscribed: true,
    oneSignalRequiresUserPrivacyConsent: false,
    platform: 'ios',
    appId: 'io.gonative.example',
    appVersion:  '1.0.0',
    distribution: 'release',
    hardware: 'armv8',
    installationId: 'xxxx-xxxx-xxxx-xxxx',
    language: 'en',
    model: 'iPhone',
    os: 'iOS',
    osVersion: '10.3',
    timeZone: 'America/New_York'
}
```

#### **Method \#2 - POST data to your API endpoint**

Configure our registration service to POST the user's push token to an API endpoint on your server. The POST request will come from the user, so you'll be able to identify which user is making the request from the headers of the request, i.e. cookies, etc.

Please specify the URL of the API endpoint you would like to receive the HTTP POST request that contains the push tokens. By default, the push tokens will be posted to your server whenever they are first generated.

You may specify a URL regex to specify other pages on which the tokens should be posted. For example, if users land on a /dashboard page after a successful login, you may want to include that URL regex so that you can then associate the device's push token with that specific authenticated user.

If you are having trouble receiving the POST, we strongly recommend specifying `.*` for testing, at least until you have verified you can receive a POST. This will POST the data on ALL URLs. If you do not receive the POST, please verify your endpoint can receive a POST request by testing with a tool like Curl or Postman.

Finally, make sure you log the entire POST request, including headers. Any session cookie that identifies the user will come across in the headers of the request. If you are using PHP, you can use something like:  
`$data = file_get_contents('php://input'); $data = json_decode($data);`

