Integrating the SDK with your project
=====================================

Gradle Integration 
------------------------------

To add the dependency, open your project and update the app module’s build.gradle to have the following repositories and dependencies:

```
allprojects {
    repositories {
      ...
      maven { url "http://sdk.prod.gcp.openx.org/" }
      ...
    }
}

// ...

dependencies {
    ...
    implementation ("com.openx:android-sdk:4.13.0@aar") {
        transitive = true
    }
    ...
}
```



Manual Integration
------------------------------


1. [Download](http://sdk.prod.gcp.openx.org/android/4.13.0/OpenX_Mobile_SDK_Android_4.13.0.zip) the OpenX Mobile Android SDK zip file.
1. [Import the SDK](android-sdk-importing.md) into your app.

Updating your Android manifest
------------------------------

Before you start, you need to integrate the SDK by updating your Android manifest.

1.  Open your AndroidManifest.xml and add the following permissions and
    activity declarations according to the bundle you are integrating.

    ``` xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    ```

    **Notes:**

    -   `ACCESS_COARSE_LOCATION` or `ACCESS_FINE_LOCATION` will
        automatically allow the device to send user location for
        targeting, which can help increase revenue by increasing the
        value of impressions to buyers.
    -   `WRITE_EXTERNAL_STORAGE` is optional and only required for MRAID
        2.0 storePicture ads.

2.  For *banner and interstitial ads only*, include the following custom
    activities (even though you won't instantiate them directly). This
    is not necessary for video interstitial ads.

    Custom Activities:

    ``` xml
     <activity
        android:name="com.openx.apollo.views.browser.AdBrowserActivity"
        android:configChanges="orientation|screenSize|keyboardHidden"
        android:theme="@android:style/Theme.Translucent.NoTitleBar"
        android:windowSoftInputMode="adjustPan|stateHidden"
        android:launchMode="singleTop"/>  
    ```

3.  Add this tag to your `<application>` to use Google Play Services:

    ``` xml
     <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />  
    ```

Initializing the SDK for video pre-caching
------------------------------------------

The initialization of the SDK occurs dynamically when a user is about to
view a video ad. As of OpenX Android SDK v4.9, you can enable
initialization with video pre-caching, which causes an ad video to load
on a device before the user views it. This alternative to streaming
ensures that the video plays without delays for a better user
experience.

> **Note:** Video pre-caching requires the OpenX v4.9 Beta SDK. Contact your account
manager if you want to be a test partner.

To enable pre-caching, you need to integrate [video interstitial
ads](android-sdk-video-interstitial-integration.md) or [opt-in video
ads](android-sdk-video-optin-integration.md) into your app. You also
need to perform a separate initialization step, as in the following
example:

``` java
// Create a container for initialization with custom options.
PreCacheAdConfiguration preCacheAdConfiguration = new PreCacheAdConfiguration();
ArrayList[String] vastUrls = new ArrayList[](); 

// Provide a list of vastURLs that to preload. 
vastUrls.add("http://mobile-d.openx.net/v/1.0/av?auid=540396661"); 
preCacheAdConfiguration.vastTags = vastUrls; 

// Set VAST as PreCacheAdConfiguration adUnitIdentifier
preCacheAdConfiguration.adUnitIdentifierType = AdConfiguration.AdUnitIdentifierType.VAST; 

try {
    // Start SDK Initialization with custom options. 
    // OpenX SDK will initiate the loading of ads for provided vast tags immediately.
    OXSettings.initializeSDK(this, this, preCacheAdConfiguration);
}
catch (AdException e) {
    Log.w(this.getClass().getSimpleName(), e.getMessage());
}
```

Now you are ready to continue with specific integration steps for each
ad type, or see the relevant adapter instructions:

-   [Banner ad integration](android-sdk-banner-integration.md)
-   [Interstitial integration](android-sdk-interstitial-integration.md)
-   [Video interstitial integration](android-sdk-video-interstitial-integration.md)
-   [Opt-in video integration](android-sdk-video-optin-integration.md)
-   [300x250 video](android-sdk-info/android-sdk-video-ad-view-integration.md) (New in 4.10)
-   [MoPub adapter](android-sdk-mopub-adapter.md)
-   [AdMob adapter](android-sdk-admob-adapter.md)
