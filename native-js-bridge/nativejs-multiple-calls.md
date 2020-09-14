# NativeJS - Multiple calls



When making multiple simultaneous calls to the NativeJS bridge, subsequent assignments of window.location.href will overwrite the earlier assignments. The result is that only the last command will be executed. For example:

```javascript
window.location.href = ... // first command
// later...
window.location.href = ... // second command overwrites the first one
```

One solution is to introduce a small \(500ms\) delay before running the second command:

```javascript
window.location.href = ... // first command

setTimeout(function() {
	window.location.href = ... // second command
}, 500);
```

The second solution is to pack both commands into a single NativeJS call to `gonative://nativebridge/multi`:

```javascript
var urls = ['gonative://...', 'gonative://...'];
var json = JSON.stringify({urls: urls});
window.location.href = 'gonative://nativebridge/multi?data=' + encodeURIComponent(json);
```

