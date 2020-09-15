# Navigation hierarchy

### **Overview**

Some native apps provide a hierarchical or multi-level navigation experience to users. See the video below for an example.

![Navigation hierarchy example](../.gitbook/assets/navigation_hierarchy.gif)

### **Configuration**

You may specify levels for each URL in your app using regular expressions. Whenever a user navigates to a URL with a higher level specified, the page will load in a new Android Activity or iOS ViewController.

Regex matches are prioritized top to bottom. If no match is found for a link, the link will open in the current level.

_Note: Navigation hierarchy will load the new page in a new webview, and requires a full page load. If your site is a single-page-app driven by AJAX, then you may need to force a full page load using something like:_ `window.location.href = 'https://my-ajax-site/path";`

### **Learn more**

* [Example JSON for BooyaFitness.com](https://gonative.io/docs/booyafitness_navStructure_urlLevels_example.json) 

