# NativeJS - Share

Run the following code via javascript to invoke the native share dialog on Android and iOS. You may also use gonative://share/sharePage in your native sidebar and tab menus.

```javascript
window.location.href = 'gonative://share/sharePage'; 
```

You may also use the following to share a URL other than the current URL:

```javascript
window.location.href="gonative://share/sharePage?url=http://example.com”; 
```

Please make sure to URL-encode the URL you use, i.e. `https://example.com` -&gt; `https%3A%2F%2Fexample.com`

