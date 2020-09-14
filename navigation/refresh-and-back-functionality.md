# Refresh & back button

### **Pull-to-Refresh**

Pull-to-refresh adds the ability for the user to refresh the current page by swiping down from the top of the screen.

On some Android apps, there is a bug where the pull-to-refresh functionality will be inadvertantly activated any time the user scrolls up. Please test on your own app to see if there's a problem, and if there is please disable the pull-to-refresh feature for your app.

### **Refresh buttons in the top toolbar**

You may also include refresh icons in your Android ActionBar and iOS Navigation Bar. Clicking these buttons will refresh the current page.

### **iOS Contextual Back Button**

This feature adds a back button to your app whenever there is a web history available. There is no need for this feature on Android, as all Android devices have a hardware back button.

If you need to change the word "Back" to another string, or restrict the visibility only to certain pages, you can edit the config directly in Import/Export section as follows.

```text
// Example 1
"toolbarNavigation": {
    "items": [{
            "system": "back",
            "title": "返回"
        }
    ]
},

// Example 2
"toolbarNavigation": {
    "items": [{
            "system": "back",
            "icon": "left"
        }
    ]
},

// Example 3
"toolbarNavigation": {
	"visibility": "anyItemEnabled",
	"items": [{
		"system": "back",
		"urlRegex": [
				".*paypal.com.*",
				".*facebook.com.*"
			]
		}
	]
}
```

