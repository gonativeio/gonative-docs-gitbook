# Native search form

### **Overview**

Configure a native search form in the top native toolbar to use your existing web search.

_Note: This feature will add an Android ActionBar and iOS Navigation Bar to your app._  


![Android native search example](https://gonative.io/docs/native_search_android.png)

![iOS native search example](https://gonative.io/docs/native_search_ios.png)

### **Configuration**

Supporting native search will place a magnifying glass icon in the Android ActionBar and iOS NavigationBar. Upon clicking the search icon, users will be able to enter `[search terms]`. These `[search terms]` will simply be appended to the Template Search URL specified.

For example, all Google.com searches can be represented using the URL syntax `https://www.google.com/#q=[search terms]`.

