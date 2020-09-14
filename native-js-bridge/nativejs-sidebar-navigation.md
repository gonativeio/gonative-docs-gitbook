# NativeJS - Sidebar navigation

You may turn on native sidebar navigation in your config, leave the menu blank, and then use the following javascript NativeJS Bridge to set the values.

```javascript
var items = [{
	label: "google",
	url: "https://google.com",
	icon: "fa-cog" // optional font awesome 4.7 icon
}, {
	label: "sample grouping",
	isGrouping: true, 
	subLinks: [{
		label: "apple",
		url: "https://apple.com",
		icon: "fa-home" // optional
	}, {
		label: "google",
		url: "https://google.com",
		icon: "fa-home" //optional
	}]
}, {
	label: "sample javascript",
	url: "javascript:alert('test')"
}];
var json = JSON.stringify(items);

window.location.href = "gonative://sidebar/setItems?items=" + encodeURIComponent(json);
```

Note that the `window.location.href` will be intercepted by your app, and nothing will actually load. Just the sidebar will be set.

