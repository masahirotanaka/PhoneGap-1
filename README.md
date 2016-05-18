# PhoneGap AppsFlyer plugin for Android and iOS. 

Built against Phonegap >= 3.3.x.
## Setup the plugin using Monaca Cloud IDE ##

1\. Add the PhoneGap AppsFlyer plugin to your project. Information about how to add/import PhoneGap/Cordova plugins can be found [here](https://docs.monaca.io/en/manual/dependencies/cordova_plugin/).

2\. Add the following xml to your `config.xml` in the root directory of your `www` folder:
```xml
<!-- for iOS -->
<feature name="AppsFlyerPlugin">
  <param name="ios-package" value="AppsFlyerPlugin" />
</feature>
```
```xml
<!-- for Android -->
<feature name="AppsFlyerPlugin">
  <param name="android-package" value="com.appsflyer.cordova.plugin.AppsFlyerPlugin" />
</feature>
```

3\. For Android, add the following xml to your `AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
<uses-permission android:name="android.permission.READ_PHONE_STATE" />
```
4\. Add new app on AppsFlyer dashboard.
Make sure that the value in the manifest and the value entered in the dashboard are identical.
If you want to tracking installs for Android-Out-Of-Store Applications, please take a look [here](https://support.appsflyer.com/hc/en-us/articles/207447023-Tracking-Installs-for-Out-Of-Store-Applications).

5\. Add following lines to your code to be able to initialize tracking with your own AppsFlyer dev key:
```javascript
document.addEventListener("deviceready", function(){
    var args = [];
    var devKey = "xxXXXXXxXxXXXXxXXxxxx8";   // your AppsFlyer devKey
    args.push(devKey);
    var userAgent = window.navigator.userAgent.toLowerCase();
                          
    if (/iphone|ipad|ipod/.test( userAgent )) {
        var appId = "123456789";            // your ios app id in app store
        args.push(appId);
    }
    window.plugins.appsFlyer.initSdk(args);
}, false);
```
6\. Test your app for [Android](https://support.appsflyer.com/hc/en-us/articles/207032136-Testing-AppsFlyer-Android-SDK-Integration-Before-Submitting-to-Google-Play) / [iOS](https://support.appsflyer.com/hc/en-us/articles/207032046-Testing-AppsFlyer-iOS-SDK-Integration-Before-Submitting-to-the-App-Store-) before submitting to the Google Play / App Store. 

## Setup the plugin using Monaca CLI ##

1\. Install Monaca CLI. How to proceed, please take a look [here](https://github.com/monaca/monaca-cli).

2\. Getting started with [Monaca CLI](https://docs.monaca.io/en/manual/development/monaca_cli/).

3\. Create a new project. 

```$ monaca create project-name```

```$ cd project-name```

4\. Remove the directory platforms

```$ monaca rm -r platforms/```

After removing the platforms/ directory add it again and choose which platform

```$ monaca platform add platform-name```

5\. Add the PhoneGap AppsFlyer plugin to your project

You can add the plugin using the URL or adding the plugin package to your project

Using the plugin URL

``` $ monaca plugin add https://github.com/monaca-plugins/com.appsflyer.phonegap.plugins.appsflyer.git``` 

Using the plugin package in your project you have to create a folder ```plugins``` to your project and copy the plugin package there.

7\. Upload the project to Monaca Cloud

```$ monaca upload```

8\. Build the project.

```$ monaca remote build```

Take a look how to build Monaca App for Android / iOS [here](https://docs.monaca.io/en/quick_start/cli/building_app/).




## Usage:

#### 1\. Set your App_ID (iOS only), Dev_Key and enable AppsFlyer to detect installations, sessions (app opens), and updates.  
**Note: ** *This is the minimum requirement to start tracking your app installs and it's already implemented in this plugin. You **_MUST_** modify this call and provide:  *
- *devKey* - Your application devKey provided by AppsFlyer.
- *appId*  - **For iOS only.** Your iTunes application id.
```javascript
document.addEventListener("deviceready", function(){
    var args = [];
    var devKey = "xxXXXXXxXxXXXXxXXxxxx8";   // your AppsFlyer devKey
    args.push(devKey);
    var userAgent = window.navigator.userAgent.toLowerCase();
                          
    if (/iphone|ipad|ipod/.test( userAgent )) {
        var appId = "123456789";            // your ios app id in app store
        args.push(appId);
    }
    window.plugins.appsFlyer.initSdk(args);
}, false);
```

#### 2\. Set currency code (optional)
```javascript
//USD is default value. Acceptable ISO(http://www.xe.com/iso4217.php) Currency codes here. Examples:  
//British Pound: window.plugins.appsFlyer.setCurrencyCode("GBP");  
window.plugins.appsFlyer.setCurrencyCode("USD");
```
#### 3\. Set customer user ID (Advance)
*Setting your own custom ID will enable you to cross-reference your own unique ID with AppsFlyer’s user ID and the 
other devices’ IDs. This ID will be available at AppsFlyer CSV reports along with postbacks APIs for cross-referencing 
with you internal IDs.*  
**Note:** *The ID must be set during the first launch of the app at the SDK initialization. The best practice is to call to this API during `deviceready` event if possible.*
```javascript
window.plugins.appsFlyer.setAppUserId(userId);
```
#### 4\. In App Events Tracking API (optional)
*These events help you track how loyal users discover your app and attribute them to specific campaign/source.*
- *These in-app events help you track how loyal users discover your app, and attribute them to specific 
campaigns/media-sources. Please take the time define the event/s you would like to measure to allow you 
to track ROI (Return on Investment) and LTV (Lifetime Value).*
- *The “trackEvent” method allows you to send in-app events to AppsFlyer analytics. This method allows you to 
add events dynamically by adding them directly to the application code.*
```javascript
// eventName - any string to define the event name. For example: “registration” or “purchase”
// eventValue - the sales value. For example: 0.99 or 0.79
window.plugins.appsFlyer.sendTrackingWithEvent(eventName, eventValue);
// window.plugins.appsFlyer.sendTrackingWithEvent(eventName, "");
```
#### 4\.1 Rich In App Events Tracking API (optional)
AppsFlyer’s rich in­app events provide advertisers with the ability to track any post­install event and attribute it to a media source and campaign.
An in­app event is comprised of an event name and event parameters

```javascript
var eventName = "af_add_to_cart";
var eventValues = {"af_content_id": "id123", "af_currency":"USD", "af_revenue": "2"};
window.plugins.appsFlyer.trackEvent(eventName, eventValues);
```
#### 5\. Get AppsFlyer’s Unique Device UID (Advanced)
*Get AppsFlyer’s proprietary device ID. AppsFlyer device ID is the main ID used by AppsFlyer in the Reports and API’s.*
```javascript
// getUserIdCallbackFn - callback function
window.plugins.appsFlyer.getAppsFlyerUID(getUserIdCallbackFn);
```
###### Example:
```javascript
var getUserIdCallbackFn = function(id) {
    alert('received id is: ' + id);
}
window.plugins.appsFlyer.getAppsFlyerUID(getUserIdCallbackFn);
```
#### 6\. Accessing AppsFlyer Attribution / Conversion Data from the SDK (Deferred 
Deep-linking). Read more: [Android](http://support.appsflyer.com/entries/69796693-Accessing-AppsFlyer-Attribution-Conversion-Data-from-the-SDK-Deferred-Deep-linking-), [iOS](http://support.appsflyer.com/entries/22904293-Testing-AppsFlyer-iOS-SDK-Integration-Before-Submitting-to-the-App-Store-)  
**Note:** AppsFlyer plugin will fire `onInstallConversionDataLoaded` event with attribution data. You must implement listener to receive the data.
###### Example:
```javascript
document.addEventListener('onInstallConversionDataLoaded', function(e){
    var attributionData = (JSON.stringify(e.detail));
    alert(attributionData);
}, false);
```
