# NativeJS - iOS location prompt

### **Disable double location prompt on iOS**

We have done a native override of the geolocation, so that only the app will ask for permission.

The one caveat is that we only override the web-based geolocation with our native one after the page has loaded. So, there is potential timing issue if you call the javascript geolocation before we do the native override.

So, the solution is that once we finish doing the native binding, we will call a javascript function `gonative_geolocation_ready();`

So, you will need to delay the location prompt until that function is called. For example:

```javascript
if (navigator.userAgent.indexOf('GoNativeIOS') > -1) {
    function gonative_geolocation_ready() {
         // call navigator.geolocation.getCurrentPosition in this function
    }
} 
else {
    // call navigator geolocation.getCurrentPosition right away directly
}
```

