# Xtremepush SDK

### Registration service and javascript info

The following fields will be added to the JSON posted to your registration endpoint. They will also be provided if you define a javascript function called `gonative_xtremepush_info`.

* `xtremepushDeviceId`: string that identifies the device in xtremepush. You can use this to send targeted push notifications.
* `xtremepushDeviceToken`: push token provided by the operating system

### NativeJS Bridge endpoints

XtremePush has the concepts of tags, events, and impressions. Each tag, event, or impression has a string name. Tags and events may also contain a string value.

To send a tag, event, or impression, open the appropriate url:

* gonative://xtremepush/hit/tag?name=tag123
* gonative://xtremepush/hit/event?name=event123
* gonative://xtremepush/hit/impression?name=impression123

You may also add a value field for tags and events, e.g.

* gonative://xtremepush/hit/tag?name=score&value=42

### Additional resources 

More documentation on registration service callback functions [here](../../push-notifications/sending-personalized-push.md)

Learn more about how to use NativeJS Bridge [here](../../native-js-bridge/nativejs-bridge-overview.md)

How to detect usage coming from your apps [http://support.gonative.io/knowledge\_base/topics/how-do-i-detect-usage-coming-from-my-apps](http://support.gonative.io/knowledge_base/topics/how-do-i-detect-usage-coming-from-my-apps)

