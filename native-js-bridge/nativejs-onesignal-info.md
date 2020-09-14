# NativeJS - OneSignal info

### **Get OneSignal push information**

If your website defines a function called gonative\_onesignal\_info, it will get called after every page load as shown below. You may then POST it to your server via AJAX, or do anything else with it. More info [here](https://gonative.io/#registration-service).

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

