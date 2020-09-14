# In-App Purchases \("IAP"\)

## Overview

In-app purchases provide additional channels to monetize your app. iOS contains these types of in-app purchases:

* Consumables: can be purchased multiple times, e.g. 100 digital coins
* Non-consumables: can only be purchased once, e.g. a particular character skin, or permanent advertising removal
* Non-renewable subscriptions: lasts for a fixed amount of time and then expires, e.g. 1-year premium access
* Renewable subscriptions: automatically renews, e.g. monthly premium access

Google Play allows for standard user-initated in-app purchases and autorenewing subscription purchases.

Generally, if your app accepts payment for any digital goods or memberships, Apple’s rules require you to accept in-app purchases as the only payment method within your mobile app. Google Play requires you to offer in-app purchases as an option for digital content, unless such content can also be consumed outside of the app. You are still free to implement other payment methods on your website separate from the mobile apps.

GoNative’s in-app purchase flow has these steps:

1.     Create IAP in iTunes Connect / Google Play Console website

2.     App gets list of purchasable items from your site and verifies with store

3.     Site shows UI for purchasable items

4.     User starts purchase.

5.     Purchase is verified and fulfilled

1. 1. If using server-side verification, app posts receipt to your site. Web server verifies receipt with Apple and fulfills purchase.
   2. If using client-side verification, purchase data is available via javascript.

## iOS Implementation

Create your app on iTunes connect, picking a globally unique bundle ID.

Open your new app on iTunes connect, go to Features -&gt; In-App Purchases.

Create your In-App purchase. The Product ID is a string used to identify the IAP. For auto-renewing types, there is a concept of "subscription groups". Users can only be subscribed to one IAP in each subscription group. For example, you may offer a monthly or semiannual membership. It would only make sense for a user to be subscribed to one, not both. There is additional information used to describe the IAP, and notes for Apple's review. You must create a user-friendly description in at least one localized language.

At the IAP screen, click "View Shared Secret". Save this string.

Create a JSON file that lists the product IDs you would like to offer for sale. This allows you to in real time add or remove products from sale without publishing a new version of your app. The following example shows two products for sale, member\_basic\_w and member\_basic\_m.

```javascript
{
    "products": [
        "member_basic_w",
        "member_basic_m"
        ]
}
```

Host the JSON file on your website.

Now you are ready to enable and configure the module. You will need:

* productsUrl - The productsUrl should point to the JSON file on your website. When your GoNative app launches, it will make an HTTP GET to your productsUrl.
* postUrl - The postUrl should be provided if you are using server-side verification \(explained below\). It can be omitted if you would like to handle the purchases on the device.

The productsUrl should point to the JSON file on your website. When your GoNative app launches, it will make an HTTP GET to your productsUrl.

postUrl should be provided if you are using server-side verification \(explained below\). It can be omitted if you would like to handle the purchases on the device.

The app will verify the list of product IDs with iTunes to ensure they are all available for purchase. Then it will execute a javascript function on your website called "gonative\_info\_ready", with a single object parameter:

```javascript
{
    inAppPurchases: {
        platform: 'iTunes',
        canMakePurchases: true,
        products: [{
            productID: 'product_id',
            localizedDescription: 'Description from iTunes',
            localizedTitle: 'Title from iTunes',
            price: 9.99,
            priceLocale: 'en-US',
            priceFormatted: '$9.99'
        }]
    }
}
```

On iOS, platform will always be iTunes. canMakePurchases may be false if disabled via parental controls, or due to other reasons \(see Testing process\). Products is an array generated from productsUrl, filtered to show only purchasable items and with additional fields added.

You should create a page on your website that shows the available items for purchase. It should wait for the gonative\_info\_ready function to be called and then populate the available items for purchase and display. Show the price using the priceFormatted string, as users may have different language and currency settings.

When a user decides to purchase an IAP, open the URL gonative://purchase/product\_id. The GoNative app will start the in-app purchase flow.

### Server-side verification

There are two methods available to fulfill your user’s purchases: server-side, and on-device. Server-side verification is generally recommended if purchases are to be associated with a user account, as it is more secure. When an in-app purchase is made, the purchase data will be sent to your web server, which will credit or fulfill the purchased item after it has verified the purchase with Apple. During this process, you should associate the purchase with the logged-in user in your system.

For example, your website may have user logins with a free membership tier, and a premium membership tier. In this case, you should only display the purchase page within the logged-in section of your website. When the purchase is made, the receipt data will be POSTed the configured postUrl with the same cookies as the logged-in user.

The JSON POST to postUrl will contain the contents:

```javascript
{
	"receipt-data": "xxxxxxxxxxxxxxxxxxx"
}

```

Your web server needs to create a post to [https://buy.itunes.apple.com/verifyReceipt](https://buy.itunes.apple.com/verifyReceipt) with the contents:

```javascript
{
    "receipt-data": "xxxxxxxxxxxxxxx",
    "password": "shared secret from iTunes connect",
    "exclude-old-transactions": true
}

```

If exclude-old-transactions is set to true, Apple will only return the latest transaction for auto-renewing subscriptions. Otherwise, you will get back the entire history of subscriptions.

Apple's server should return HTTP status 200 with a JSON object \(see example below\). If the JSON object is `{"status":21007}`, the receipt was generated from the sandbox/test environment. In that case, re-do the POST to the following url:  [https://sandbox.itunes.apple.com/verifyReceipt](https://sandbox.itunes.apple.com/verifyReceipt).

Assuming the response from Apple’s server has status 0, verify the receipt's bundle\_id matches your app, and what products have been purchased. Additionally, save the receipt-data in your database so that you can verify successful auto-renews. The receipt-data serves as a “token” you can use to get updated subscription information. At this point, your server should provide whatever it is the user has purchased \(premium content, virtual currency, etc.\)

Your server should respond with a JSON object:

```javascript
{  
	"success": true,
	  "title": "Thank you for your purchase!",
	"message": "Your IAP has been credited to your account",
	"loadUrl": "https://example.com/purchase-success"
}
```

The GoNative app will notify iTunes that the purchase has been fulfilled and show the user your message. “loadUrl” is an optional field, which the app will open and show to the user if received.

If the status in the JSON is any value other than 0, or Apple’s endpoint does not return an HTTP status 200, or the request to Apple fails, do not fulfill the purchase. See [https://developer.apple.com/documentation/appstorereceipts/status](https://developer.apple.com/documentation/appstorereceipts/status) for other possible JSON status values. Your web server should respond with a JSON object with success set to false. We recommend logging the response from Apple for troubleshooting purchases, especially the status field.

On failure, your web server may supply a message, title, and loadUrl to provide feedback to your user. You may choose to surface the status value from Apple to your user in a message. The app will re-attempt the POST to your web server each time it launches until it gets a success. If a purchase is not ever fulfilled, Apple will eventually refund the user.

An example response from iTunes will look as follows:

```javascript
{
    "status": 0,
    "environment": "Sandbox",
    "receipt": {
        "receipt_type": "ProductionSandbox",
        "adam_id": 0,
        "app_item_id": 0,
        "bundle_id": "io.gonative.ios.dev",
        "application_version": "1.0.0",
        "download_id": 0,
        "version_external_identifier": 0,
        "receipt_creation_date": "2016-12-01 22:26:23 Etc/GMT",
        "receipt_creation_date_ms": "1480631183000",
        "receipt_creation_date_pst": "2016-12-01 14:26:23 America/Los_Angeles",
        "request_date": "2016-12-02 02:31:44 Etc/GMT",
        "request_date_ms": "1480645904550",
        "request_date_pst": "2016-12-01 18:31:44 America/Los_Angeles",
        "original_purchase_date": "2013-08-01 07:00:00 Etc/GMT",
        "original_purchase_date_ms": "1375340400000",
        "original_purchase_date_pst": "2013-08-01 00:00:00 America/Los_Angeles",
        "original_application_version": "1.0",
        "in_app": [{
            "quantity": "1",
            "product_id": "member_basic_m",
            "transaction_id": "1000000255553673",
            "original_transaction_id": "1000000255553673",
            "purchase_date": "2016-12-01 22:26:22 Etc/GMT",
            "purchase_date_ms": "1480631182000",
            "purchase_date_pst": "2016-12-01 14:26:22 America/Los_Angeles",
            "original_purchase_date": "2016-12-01 22:26:23 Etc/GMT",
            "original_purchase_date_ms": "1480631183000",
            "original_purchase_date_pst": "2016-12-01 14:26:23 America/Los_Angeles",
            "expires_date": "2016-12-01 22:31:22 Etc/GMT",
            "expires_date_ms": "1480631482000",
            "expires_date_pst": "2016-12-01 14:31:22 America/Los_Angeles",
            "web_order_line_item_id": "1000000033828184",
            "is_trial_period": "false"
        }]
    },
    "latest_receipt_info": [{
            "quantity": "1",
            "product_id": "subscription1",
            "transaction_id": "1000000255546758",
            "original_transaction_id": "1000000255546758",
            "purchase_date": "2016-12-01 21:28:05 Etc/GMT",
            "purchase_date_ms": "1480627685000",
            "purchase_date_pst": "2016-12-01 13:28:05 America/Los_Angeles",
            "original_purchase_date": "2016-12-01 21:28:05 Etc/GMT",
            "original_purchase_date_ms": "1480627685000",
            "original_purchase_date_pst": "2016-12-01 13:28:05 America/Los_Angeles",
            "is_trial_period": "false"
        }
    }
}

```

### On-device verification

An alternative to server-side verification is on-device verification. This can be used if your app does not have user login accounts to keep track of users using different devices. It is less secure than server-side verification, as someone with a jailbroken iPhone could theoretically modify your app and make it act as if a purchase has been made.

To use on-device verification only, do not provide a postUrl in your app’s config file. When your app launches and when purchases are made, the app will execute a javascript function you have defined called gonative\_iap\_purchases with the following data:

```javascript
{
  "hasValidReceipt": true,
  "platform": "iTunes"
  "activeSubscriptions": ["member_basic_w"],
  "allPurchases": [
    {
      "purchaseDateString": "2019-08-11T15:53:13Z",
      "transactionIdentifier": "1000000556506948",
      "webOrderLineItemID": 1000000046196920,
      "originalPurchaseDateString": "2019-08-07T23:46:15Z",
      "quantity": 1,
      "productIdentifier": "member_basic_w",
      "originalTransactionIdentifier": "1000000555471857",
      "cancellationDateString": "",
      "subscriptionExpirationDateString": "2019-08-11T15:56:13Z"
    },
    {
      "purchaseDateString": "2019-08-11T16:03:44Z",
      "transactionIdentifier": "1000000556507336",
      "webOrderLineItemID": 1000000046196984,
      "originalPurchaseDateString": "2019-08-07T23:46:15Z",
      "quantity": 1,
      "productIdentifier": "member_basic_w",
      "originalTransactionIdentifier": "1000000555471857",
      "cancellationDateString": "",
      "subscriptionExpirationDateString": "2019-08-11T16:06:44Z"
    }
  ]
}
```

Check that hasValidReceipt=true. The allPurchases field is an array of objects containing information on what that user’s device has purchased. For convenience, we provide an activeSubscriptions array that lists the product IDs of what subscriptions are currently active.

Parse the data in your gonative\_iap\_purchases javascript function and provide any appropriate functionality. For example, you may choose to show ads in your app if the user has not purchased a premium add-removal nonconsumable purchase or recurring subscription.

### Restoring Purchases

If you are offering subscriptions or non-consumable in-app purchases, you should add a “Restore Purchases” button somewhere in your app. This allows your users to continue to use their previous purchases if they change devices or have multiple devices.

To restore purchases, open the url `gonative://iap/restorePurchases`

Any previous purchases will be restored and will start the verification process outlined above, as if they were newly purchased. If there are no purchases to restore, no action is performed. Unfortunately, it is not possible to differentiate between a lack of purchases to restore, a delay, or a failure to restore. You may choose to display a message such as “Restore requested. If you have any previous purchases, they will be available shortly.”

### Auto-renewable Subscriptions

Apple will automatically bill users who have purchased auto-renewable subscriptions. To check on the status of a user’s subscription, POST the receipt again to Apple’s endpoint and check the latest\_receipt\_info field. You may wish to set up a regular job to go through all active subscriptions.

Apple can also notify you of subscription status changes by posting to an endpoint you have set up to handle the change events. Go to App Store Connect -&gt; Your App -&gt; App Information and enter the URL. See the “Status Update Notifications” section in the In-App Purchase Programming guide: [https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Chapters/Subscriptions.html](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Chapters/Subscriptions.html)

### Additional References:

[https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html](https://developer.apple.com/library/archive/releasenotes/General/ValidateAppStoreReceipt/Chapters/ValidateRemotely.html)

[https://help.apple.com/app-store-connect/\#/dev0067a330b](https://help.apple.com/app-store-connect/#/dev0067a330b)

### Testing process

To test your in-app purchases, you will need to create iTunes sandbox users under the App Store Connect account that owns your app. The users must not already exist in the Apple system, i.e. you cannot use your regular account logins. You can enter fake data for most of the fields but may need to set passwords that are at least 10 characters with uppercase, lowercase, and numbers.

When the app launches or a purchase is initiated, you will be prompted to sign in with the test user’s account. The purchase flow can be tested without real payment. Auto-renewing subscriptions will renew at an accelerated rate \(5 minutes per month\) for 6 times, and will then cancel.

If you are having trouble testing iOS purchases, especially if “canMakePurchases” is false, please check the following items:

* Ensure you are running the latest non-beat Xcode version
* Do not use a sandbox login to sign in directly on your device. Only enter it when the in-app purchase is prompted for on your device
* Make sure that you are using a sandbox login created under your account on iTunes Connect
* Verify that the “In-app purchase” capability has been added to your app in Xcode. Open the GoNativeIOS project -&gt; GonativeIO target -&gt; Signing & Capabilities.
* Verify that parental controls are not activated that may prevent purchases.
* Reboot your device

## Android Implementation

Create your app in the Google Play Console. The package name of your app is a globally unique identifier that identifies your app on Google Play. Your account "claims" that package name the first time you upload an APK to the Google Play Console. Make sure that any pages with grey checkmarks are completed so that the checkmark turns green. The app must be fully “set up” before you can create and test in-app purchases.

In the Google Play Console, go to Store presence -&gt; In-app products. Create either a Managed product \(standard in-app purchase\), or a Subscription. The product id will be used later in code to identify the item. Title and description are used in Google Play listings, and will also be passed to your app. If you wish to have different tiers of subscriptions \(e.g. a monthly and annual subscription\), they must be created as separate subscriptions.

Create a JSON file that lists the product IDs you would like to offer for sale. This allows you to in real time add or remove products without publishing a new version of your app. The following example shows three products for sale: a regular in-app purchase, and two subscription products. The product IDs for each must be placed in the correct section: inappProducts vs subProductions.

```javascript
{
    "inappProducts": ["remove_ads"],
    "subProducts": ["member_basic_w", "member_basic_m"]
}
 
```

Host the JSON file on your web server and make it publicly accessible.

Now you are ready to enable and configure the module. You will need:

* productsUrl - The productsUrl should point to the JSON file on your website. When your GoNative app launches, it will make an HTTP GET to your productsUrl.

The app will verify the list of products with Google Play and retrieve each product's information. Then it will execute a javascript function on your website called "gonative\_info\_ready" with the following object:

```javascript
{
    "inAppPurchases": {
        "platform": "GooglePlay",
        "products": [{
            "productID": "remove_ads",
            "type": "inapp",
            "price": "$0.99",
            "price_amount_micros": 990000,
            "price_currency_code": "USD",
            "title": "Remove ads",
            "description": "Enjoy an ad-free experience"
        }, {
            "productID": "member_basic_w",
            "type": "subs",
            "price": "$0.99",
            "price_amount_micros": 990000,
            "price_currency_code": "USD",
            "subscriptionPeriod": "P1W",
            "title": "Weekly Membership",
            "description": "Access member content (autorenews weekly)"
        }]
    }
}

```

Each product will have the following fields:

* productID: the identifier set in Google Play that matches those in the productsUrl.
* type: "inapp" or "subs"
* price: the price you should display to your user. It will be localized to their currency.
* price\_amount\_micros: the price in micro-units, e.g. 1 USD = 1000000 micros.
* price\_currency\_code: the 3-letter currency code
* title: from Google Play Console
* description: from Google Play Console

You should create a page on your website that shows the available items for purchase. It should wait for the gonative\_info\_ready function to be called and then populate the items for purchase. The price must be shown using the "price" string, as different users may have different language and currency settings.  
  
 When a user decides to purchase an IAP, open the URL gonative://purchase/product\_id. The app will start the purchase flow.

The gonative://purchase/product\_id call supports these additional query parameters for autorenewing subscriptions:

* previousProductID – the product ID we are replacing. This is required to support subscription upgrades and downgrades \(i.e. converting a monthly subscription to annual, or changing a subscription tier from basic to premium membership\).
* prorationMode – determines how to credit the user for a mid-period subscription change. Valid values include:
  * IMMEDIATE\_WITH\_TIME\_PRORATION – Replacement takes effect immediately, and the remaining time will be prorated and credited to the user. This is the current default behavior.
  * IMMEDIATE\_AND\_CHARGE\_PRORATED\_PRICE - Replacement takes effect immediately, and the billing cycle remains the same. The price for the remaining period will be charged. This option is only available for subscription upgrade.
  * IMMEDIATE\_WITHOUT\_PRORATION – Replacement takes effect immediately, and the new price will be charged on next recurrence time. The billing cycle stays the same.
  * DEFERRED - Replacement takes effect when the old plan expires, and the new price will be charged at the same time.

`gonative://purchase/annual_membership?previousProductID=monthly_membership&prorationMode= IMMEDIATE_AND_CHARGE_PRORATED_PRICE`

### On-device verification

On app launch, and after any purchases are made, the app will call a javascript function on your page named gonative\_iap\_purchases with a single object parameter. Here is an example object:

```javascript
{
  "platform": "GooglePlay",
  "allPurchases": [
    {
      "orderId": "GPA.3309-4129-7588-25875",
      "packageName": "io.gonative.android",
      "productID": "member_basic_w",
      "purchaseTime": 1567462252415,
      "purchaseState": 0,
      "purchaseToken": "fffpnbenegliokdcifadiihi.AO-J1OzGRezs5VkyKoyhYb-HgLEVG5XxswFLcLOyAnyy48sQPii2Yf6JYJe-Hm44FZT7ctkkkTlhRat15hoBWMnwXPzSgzlnCaYOvFRI_Yk5bzrBLXwOW-Mad1j9NsdoYkNywrOmJDzJ",
      "autoRenewing": true,
      "acknowledged": true,
      "purchaseTimeString": "2019-09-02T22:10:52.415Z",
      "purchaseStateString": "purchased"
    }
  ]
}

```

Each purchase item in the allPurchases array may have these fields:

* orderId: identifies the purchase transaction
* packageName: should match the packageName of your app
* productID: the identifier for what was purchased
* purchaseTime: milliseconds since the unix epoch \(Jan 1 1970\)
* purchaseTimeString: formatted as a string
* purchaseState: 0 – purchased, 1 – canceled, 2 – pending
* purchaseString: “purchased”, “canceled”, “pending”, or “unknown”
* acknowledged: indicates the app has confirmed the purchase with Google Play
* autoRenewing: indicates the purchase will autorenew. Note that this will be set to false if the user cancels their subscription.
* purchaseToken: a string that can be used to verify the purchase with Google

The allPurchases array will list all current subscriptions. Any expired subscriptions will not longer appear.

### Subscription management

You may wish to provide links for your users to manage their subscriptions. Open the url gonative://iap/manageAllSubscriptions to open the Google Play page that lists all subscriptions for all of the user’s apps \(not just your app\). Open the url gonative://iap/manageSubscription?productID=PRODUCTID to allow the user to manage just your app’s subscription with the specified product ID.

## Testing process

Testing your in-app purchase flow requires your app be properly set up in Google Play to be distributed.

Your test devices must be signed into a Gmail or Google Apps for Business account. You may use your day-to-day account. In your Developer Account -&gt; Account details, go to the License Testing section. Add the email addresses for the test accounts.

On your app’s management page, create an internal test track. Add the test user email to a user list. Once added, each test user go to the Opt-in URL \(looks like [https://play.google.com/apps/internaltest/1234321234](https://play.google.com/apps/internaltest/1234321234)...\) and accept the Opt-In. Create a release build \(or use the GoNative-built apk\) and upload it to the internal test track. The test users should then be able to find the app in the Google Play Store on their devices and install it.

Subsequent releases to the internal test track will immediately be available to the test devices by checking for new updates in the Google Play Store app.

Any in-app purchases can be tested without actual payment being exchanged. Auto-renewing subscriptions will renew every 5 minutes until they are canceled.

References:

[https://developer.android.com/google/play/billing/billing\_overview](https://developer.android.com/google/play/billing/billing_overview)

[https://medium.com/bleeding-edge/testing-in-app-purchases-on-android-a6de74f78878](https://medium.com/bleeding-edge/testing-in-app-purchases-on-android-a6de74f78878)

