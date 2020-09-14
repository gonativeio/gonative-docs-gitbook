# Native sidebar navigation

### Overview

Configure your app to use a native sidebar navigation menu. Users access this navigation menu through a toggle button in the top left of your app, or by sliding from the left side of the screen.

_Note: This feature will add an Android ActionBar and iOS Navigation Bar to your app._

### Visual

![Android sidebar navigation example](https://gonative.io/docs/native_navigation_android.png)

![iOS sidebar navigation example](https://gonative.io/docs/native_navigation_ios.png)

### Configuration

After entering an Initial URL, we will automatically crawl your site and attempt to pull out various navigation menu options. Please select from one of the pre-populated options, or click "Start from scratch".

Next, enter labels and absolute URLs for each menu option. You may also re-order links, add groupings, and specify icons.

If you'd prefer your links to trigger various javascript commands, you may wish to specify URLs with the following syntax:

```text
javascript:alert('test');
```

A more complicated example is to click an element when it exists, otherwise load a new URL.

```text
javascript:if (typeof jQuery === 'undefined' || jQuery('#linkElement').length == 0) window.location = 'http://example.com/target'; else jQuery('#linkElement').click();
```

### Grouping

You may click and drag a menu item under a grouping. You may also add the item to the subLinks array directly in the JSON. When the item is added to the grouping, it will be indented from the left, and appear under the subLinks when looking at the JSON.

![Grouping example](https://gonative.io/images/docs/grouping.gif)

### **Learn more**

* [Android Navigation Drawer Documentation](https://material.io/components/navigation-drawer#anatomy)
* [iOS Sidebar Library used by GoNative](https://github.com/romaonthego/REFrostedViewController)
* [Example JSON for BooyaFitness.com](https://gonative.io/docs/booyafitness_native_navigation_example.json)
* [Example JSON using Javascript \(Advanced\)](https://gonative.io/docs/stripe_native_navigation_example.json)

