# AdMob native ads SDK

_Once the premium module has been added to your app, you may use the following APIs to access its functionality._

## Overview

The GoNative.io AdMob module integrates mobile advertisements into your app. There are two types of ads: banner ads and interstitials.

Banner ads are shown near the bottom of the app’s window, immediately below the webview and above any toolbars or tab bars. They are intended to be constantly displayed while your app is being used.

Interstitial ads appear full-screen and interrupt your user’s activity. They typically are animated, may play video, and may have a timer of a few seconds that prevents the user from immediately closing the ad. Because interstitial ads are more intrusive, they should be used and activated with care.

## Enabling the module

In order to configure the AdMob module for your app, you will need to provide the bannerAdUnitId and the interstitialAdUnitId for both Android and iOS. 

There are separate unit identifiers for Android and iOS because Admob considers them to be two separate apps.

You will also provide the applicationId using the value from the AdMob website for your app. Create ad units for a banner ad and an interstitial ad. If interstitialAdUnitId or bannerAdUnitId is not provided, then the respective ad type will not be shown.

If bannerAdUnitId is provided, the banner ads will automatically be shown when your app launches. Configure its text styling, refresh rate, etc via the AdMob dashboard.

You may set a field called “showBanner” to false to disable automatically showing the banner ad if you prefer only to show it programmatically \(see below\).

## Implementation Guide

Once the premium module has been added to your app, you may use the following APIs to access its functionality.

Interstitial ads are not shown automatically. They are pre-loaded in the background and must be invoked afterwards.

To show an interstitial ad, open the url `gonative://admob/showInterstitialIfReady`. This should be done when the user is at a natural pausing point, for example when they tap something to initiate an action, or they have finished viewing a piece of content. Note that interstitial ad fill rates are significantly lower than banner ads, and there may not always be an interstitial ad ready. In that case, no interstitial will be shown.

Alternatively, open the url `gonative://admob/showInterstitialOnNextPageLoadIfReady`. The interstitial ad, if available, will be shown the next time the user navigates to a new page.

Open the urls `gonative://admob/banner/enable` and `gonative://admob/banner/disable` to enable and disable the banner ad.

