# NativeJS - Explicit push consent

### **Explicitly granting OneSignal push consent**

With this configuration, you can require users to explicitly consent to push notifications before they are used in your app.

In Import/Export section when editing your app through our website, or alternatively in appConfig.json file directly if modifying source code, please add a new field under services -&gt; oneSignal -&gt; "requiresUserPrivacyConsent" : true. The default is false.

```javascript
"oneSignal": {
    "active": true,
    "applicationId": "XXXXXXXX",
    "requiresUserPrivacyConsent": true
}
```

If requiresUserPrivacyConsent is true, user will need to explicitly consent before any device data is sent to OneSignal to support push notifications.

You may then trigger the prompt for push notification consent by opening url `gonative://onesignal/userPrivacyConsent/grant`, or running javascript `window.location.href = 'gonative://onesignal/userPrivacyConsent/grant';`

To revoke consent, please use url `gonative://onesignal/userPrivacyConsent/revoke`

We have also added to gonative\_onesignal\_info a new field: oneSignalRequiresUserPrivacyConsent. More info at [link](https://gonative.io/docs#native-js-onesignal-info). It will be true if consent has not yet been granted.

