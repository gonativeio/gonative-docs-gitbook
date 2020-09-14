# NativeJS - Android location

### **Prompt user to enable Location Services on Android**

Android users may have disabled Location Services on their device. To prompt the user to enable location services, run the following javascript:

```javascript
window.location.href = 'gonative://geoLocation/promptAndroidLocationServices';
```

If location services are already enabled, the function will have no effect. If they are disabled, the user will be shown a dialog asking them to enable location services. Note that this is independent from your app's location permission.

