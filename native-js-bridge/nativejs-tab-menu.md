# NativeJS - Tab menu

To select tabs:

```javascript
if (navigator.userAgent.indexOf('gonative') > -1) {
window.location.href = 'gonative://tabs/select/1';
}
```

Note that the tabs are 0-indexed, i.e. tabs/select/1 will select the second tab

To change the tabs:

```javascript
var tabs = {
    "enabled": true,
    "items": [{
        "icon": "fa-cloud",
        "label": "Tab 1",
        "url": "javascript:alert('You selected tab 1')"
    }, {
        "icon": "fa-globe",
        "label": "Tab 2",
        "url": "javascript:alert('You selected tab 2')"
    }, {
        "icon": "fa-users",
        "label": "Tab 3",
        "url": "javascript:alert('You selected tab 3')"
    }]
};

var json = JSON.stringify(tabs);
window.location.href='gonative://tabs/setTabs?tabs=' + encodeURIComponent(json);
```

To hide the tabs, set "enabled" to false.

