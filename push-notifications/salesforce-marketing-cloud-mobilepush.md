# Salesforce Marketing Cloud MobilePush

###  Prompt user to allow push notifications

`window.location.href=“gonative://mobilepush/register?callback=callback”`

Callback will be called with a result object containing the success key

### Subscribe to new push notification events

`window.location.href=“gonative://mobilepush/subscribe?callback=callback”`

NOTE: This also works in the event of cold start.

Callback will be called each time a new notification is tapped. The payload will contain all custom keys sent via the push notification.

### Set Contact Key for a user

`window.location.href=“gonative://mobilepush/contactKey/set?contactKey=user@email.com&callback=callback”`

### Set tags for a user's device

`var tags = [“tag1”, “tag2”, “tag3”]`

`window.location.href=“gonative://mobilepush/tags/set?callback=callback&tags=” + encodeURI(JSON.stringify(tags))`

### Retrieve tags

`window.location.href=“gonative://mobilepush/tags/get?callback=callback”`

### Set attributes for a user's device

`var attrs ={ “attr1”: “value 1”, “attr 2”: “value 2” }`

`window.location.href=“gonative://mobilepush/attributes/set?callback=callback&attributes=” + encodeURI(JSON.stringify(attrs))`

### Retrieve attributes

`window.location.href=“gonative://mobilepush/attributes/get?callback=callback”`

### Custom notification sound

A custom notification sound can be configured by [embedding an audio file within the mobile app](https://salesforce-marketingcloud.github.io/MarketingCloudSDK-iOS/push-notifications/custom-sound.html) and [enabling Custom Sounds within Marketing Cloud MobilePush](https://help.salesforce.com/articleView?id=mc_mp_custom_sound.htm&type=5#customSound). 

