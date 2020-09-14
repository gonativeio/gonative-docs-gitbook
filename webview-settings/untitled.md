# Support window.open\(\)

**Support window.open\(\) - Required for some social logins**

When enabled, your app will handle window.open\(\) javascript similar to a browser by opening a new tab. This is required to support certain social login flows.

When disabled, the URLs specified in window.open\(\) will load in the current Webview as if the user clicked on a regular link.

Note that this will make target="\_blank" links open the same way, and that [internal vs. external link rules](https://gonative.io/#internal-vs-external-links) will not be applied to these links.

