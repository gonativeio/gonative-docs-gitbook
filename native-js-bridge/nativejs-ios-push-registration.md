# NativeJS - iOS push registration

### **Delaying Push Notification Registration on iOS**

If your iOS app supports push notifications, it will prompt the user to allow push on the first launch of your app. You may optionally delay the prompt, and trigger it manually via javascript.

To disable automatic registration, please edit the "oneSignal" object under Import/Export section when editing your app. You may also edit appConfig.json file in the source code directly. Please add `"autoRegister": false` as follows:

```javascript
"oneSignal": {
    "active": true,
    "applicationId": "XXXXXXXX",
    "autoRegister": false
}
```

You may then trigger the prompt for push notifications by opening url `gonative://onesignal/register`, or running javascript `window.location.href = 'gonative://onesignal/register';`.

Note that Android continues to auto-register, as there is no user prompt.

