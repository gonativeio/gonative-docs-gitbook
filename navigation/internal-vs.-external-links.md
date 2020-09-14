# Internal vs. external links

### **Overview**

Use this configuration to specify which URLs should be loaded internally within your app, and which should launch the user out of the app and open in their default mobile browser.

For example, if you're using Facebook Connect, you'll likely want the authentication URL to load internally, but perhaps Facebook Pages to load externally.

### **Configuration**

Our default configuration works well for most clients. You may modify this set of rules as needed.

Regex matches are prioritized top to bottom, so it's best to position the most specific rules first, and the most general rules last.

