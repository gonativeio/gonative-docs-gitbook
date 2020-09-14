# NativeJS - Navigation hierarchy

Create your navigation levels structure, JSON.stringify it, and open the url:  
`gonative://navigationLevels/set?persist=true&data=JSON`

For example:

```javascript
var json = JSON.stringify({
  active: true,
  levels: [{
    regex: '.*gonative.*',
    level: 2
  }, {
    regex: '.*',
    level: 1
  }]
});

window.location.href = 'gonative://navigationLevels/set?persist=true&data=' + encodeURIComponent(json);
```

Setting `persist=true` will save the navigation levels for use the next time the app is launched, otherwise it will only take effect for the current instance. To delete the navigation levels and revert to the appConfig.json definition, open the url:  
`gonative://navigationLevels/set?persist=true`

