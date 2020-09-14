---
description: 'Open links in your app, not the mobile browser'
---

# Universal links

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

