# Apple App Store

### **Apple Developer Account**

You must have a valid Apple Developer Account in order to publish your app to the Apple App Store.

Learn more at [https://developer.apple.com/programs/enroll/](https://developer.apple.com/programs/enroll/).

One registered, you'll need to generate your signing certificates. Learn more at [Maintaining Your Signing Identities and Certificates](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html).

### **AppStoreConnect App Store Listing**

Create your app store listing at [https://appstoreconnect.apple.com](https://appstoreconnect.apple.com/). You'll need to fill out several pieces of information including app name, description, search keywords, screenshots for the various devices sizes, content rating, contact person and a few more.

You will see a place to select the build you want to submit to the App Store. Most likely, there won't yet be a build you are able to select, as you have not yet uploaded one. Please see the next step below.

### **Uploading Your Build**

You may download the complete iOS source code for your app from you GoNative app's manage page. Uncompress the `ios_source.tar.gz` file \(you might use `tar -xzvf ios_source.tar.gz` using Terminal on Mac OSX\). Then, open `GoNativeIOS.xcworkspace` in the latest non-beta version of Xcode.

You'll want to be sure that your developer account is added to Xcode, and that the Provisioning Profile you generated previously is available to use. You may double check that your app's Bundle Id is set correctly in your app's General settings.

You can now go to Product -&gt; Archive in the menus. If Archive is grayed out, make sure to select Generic iOS Device or a plugged-in physical iOS Device in the top left of the Xcode window. Once your app is Archived, you'll see a button to upload the build to the App Store. After the upload is successful, it sometimes takes an hour before the build appears in your AppStoreConnect app store listing.

Once your build is selected, and all your App Store listing data is filled out, you may click the "Submit for Review" button in the top right. On the next page, you may be asked questions about Cryptography, the Advertising Identifier, and 3rd Party Content. GoNative apps DO use Cryptography, but fall under the exception for standard user authentication. GoNative does NOT use the Advertising Identifier. You will have to determine for yourself if your app uses 3rd party content, and if you have the rights to it.

