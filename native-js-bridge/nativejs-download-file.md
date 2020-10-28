# NativeJS - Download file

Run the following code via JavaScript to initiate download of the file at the specified URL.

```javascript
window.location.href = 'gonative://share/downloadFile?url=<URL>'; 
```

On iOS, the file is downloaded and passed to the "open with" system dialog.

On Android, the file is downloaded to _sdcard/Downloads_ if the [WebView setting for external storage](../webview-settings/android-webview-permissions.md) is enabled and the user gives permissions. Otherwise, it is downloaded internally and the app then shows an "open with" dialog.

