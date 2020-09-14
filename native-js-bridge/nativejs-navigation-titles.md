# NativeJS - Navigation titles

Create your navigation title structure, and JSON-stringify it, then open the url:  
`gonative://navigationTitles/set?persist=ture&data=JSON`

For example:

```javascript
var json = JSON.stringify({
  active: true,
  titles: [{
    title: 'my title',
    regex: '.*'
  }];
});
window.location.href = 'gonative://navigationTitles/set?persist=true&data=' + encodeURIComponent(json);
```

Setting `persist=true` will save the navigation titles for use the next time the app is launched, otherwise it will only take effect for the current instance.

To delete the navigation titles and revert to the appConfig.json definition, open the url:  
`gonative://navigationTitles/set?persist=true`

To do a one-off setting of the current page's title, url encode your title and open the url:  
`gonative://navigationTitles/setCurrent?title=URLENCODEDTITLE`

For example:

```javascript
window.location.href='gonative://navigationTitles/setCurrent?title=Hello%20World';
```

