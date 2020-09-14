# NativeJS - Device info

### **Get Device Info**

If your website defines a function called gonative\_device\_info, it will get called after every page load as shown below. You may then POST it to your server via AJAX, or do anything else with it.

```javascript
function gonative_device_info(info) {
    console.log(info);
}

// info will look like
{
    platform: 'ios',
    appId: 'io.gonative.example',
    appVersion:  '1.0.0',
    appBuild: '1.0.0' // will be appVersionCode (number) on Android
    distribution: 'release',
    hardware: 'armv8',
    installationId: 'xxxx-xxxx-xxxx-xxxx',
    language: 'en',
    model: 'iPhone',
    os: 'iOS',
    osVersion: '10.3',
    timeZone: 'America/New_York',
    isFirstLaunch: false // first time the app is launched
}
```

