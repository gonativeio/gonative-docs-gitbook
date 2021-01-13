# Native tab navigation

### **Overview**

Configure your app to use a native tab menu, with optional icons and labels. Tabs appear at the bottom of the screen on both Android and iOS.

### **Selecting tabs**

Tabs will automatically be selected when clicked.

Sometimes, a link will be clicked on your website, and this linked page coordinates to a specific tab item. For that case, you may specify a "regex" field for each tab link in the JSON, which will select that tab whenever that page loads.

![Regex tab selection example](https://gonative.io/docs/tabs_regex.png)

### **Configuration**

Similar to Sidebar Navigation, you may specify icons, labels, and links to include in each tab menu. You may then specify for which URLs the tab menu will appear by entering a regular expression to match page URLs. Use ".\*" to match all URLs so that your tabs will always appear.

If you'd prefer your links to trigger various javascript commands, you may wish to specify URLs with the following syntax:

```text
javascript:alert('test');
```

A more complicated example is to click an element when it exists, otherwise load a new URL.

```text
javascript:if (typeof jQuery === 'undefined' || jQuery('#linkElement').length == 0) window.location = 'http://example.com/target'; else jQuery('#linkElement').click();
```

### Native JS Bridge

You may also modify selection, as well as Tabs configuration as a whole, with Javascript on your website using NativeJS Bridge.

{% page-ref page="../native-js-bridge/nativejs-tab-menu.md" %}

### **Learn more**

* [Android Tabs Visual & Documentation](http://developer.android.com/design/building-blocks/tabs.html)
* [iOS Tab Bar Visual & Documentation](https://developer.apple.com/ios/human-interface-guidelines/ui-bars/tab-bars/)

