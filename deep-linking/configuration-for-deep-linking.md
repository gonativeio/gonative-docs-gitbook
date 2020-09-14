# Configuration for deep linking

With deep linking enabled, when users click a link to your website on their mobile device, it will be opened and handled within your app rather than by the user's mobile browser.

### Universal links

When deep linking is enabled, and a user clicks a link on their device matching a domain specified below, the user will be prompted to open the link within your app rather than in the mobile browser.

For Android, this will work automatically with no further configuration.

For Apple, please follow the three requirements below:

* You must create an explicit app id in developer.apple.com for your app's bundle id
* In your provisioning profile associated with that app id, you must add "Associated domains" under App Services
* You will need to add a configuration file to your website at [/apple-app-site-association](https://gonative.io/apple-app-site-association) to prove you control the referenced domain

```javascript
{
    "applinks": {
        "apps": [],
        "details": [{
            "appID": "TEAMID.io.gonative.example",
            "paths": ["*"]
        }]
    }
}
```

TEAMID comes from Apple and identifies the developer account. Learn more at [Apple Documentation](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/UniversalLinks.html).

_NOTE: Deep Links on iOS will not work unless a PATH is specified as part of the URL. For example, http://example.com/ will NOT work, but http://example.com/PATH will work._

![](https://gonative.io/images/docs/associated_domains.png)

Most likely, you will want to add your hostname with and without the 'www' prefix. For example, to support Deep Links on GoNative.io, you would add 'gonative.io', and 'www.gonative.io'.

### URL Schemes & Verified App Links

iOS URL Schemes and Android Verified App Links allow the specified URLs to open automatically in your app, without requiring the user to select the app. This is helpful in login redirect flows, or for a more seamless user experience. 

* [Android Verified App Links](https://developer.android.com/training/app-links/verify-site-associations)
* [iOS URL Schemes](https://developer.apple.com/documentation/xcode/allowing_apps_and_websites_to_link_to_your_content/defining_a_custom_url_scheme_for_your_app)

