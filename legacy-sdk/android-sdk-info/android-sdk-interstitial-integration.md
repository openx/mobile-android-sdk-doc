Interstitial integration
========================

# Prerequisites

Before integrating an interstitial ad in your app, please do the
following:

1.  Either [create a mobile ad unit](https://docs.openx.com/Content/publishers/inventory_createmobilead.html)
    or make sure you know which existing ad unit to use.
2.  Update your [AndroidManifest](android-sdk-integration.md).

For a complete example, see `InterstitialActivity.java` in the
`AndroidSDKDemo` workspace provided in the SDK zip file.

Loading interstitial ads in your app
---------------------------------------------

1.  Create and display an interstitial ad.
2.  Add a listener and implement those callbacks for the notification
    for various events.

    In your `Activity`'s `onCreate()` method, instantiate the `InterstitialView`
    using the context and the ad unit ID.
    ```java
        interstitialView = new InterstitialView(this, adDetails.adDomain, adDetails.adId, InterstitialView.AdType.HTML);
    ```
3.  Next, register your activity as the `AdEventsListener`:
    ``` java
        // Remember that "this" refers to your current activity. 
        interstitialView.addAdEventListener(this);
        // where interstitialView is an instance of InterstitialView.
        // this - the class implementing AdEventsListener.
     ```                       

4.  Now you\'re ready to display an ad. Displaying the ad involves these
    steps:

    a.  In your `onCreate()` method, call `load()` to begin caching the
        ad. It's important to fetch interstitial ad content well before
        you plan to show it, since it often incorporates rich media and
        may take some time to load. OpenX recommends caching when your
        `Activity` is first created, but you may also choose to do it
        based on events in your app, like beginning a game level.

        The `load()` method will take a few seconds, as it requests and
        caches ad content, including JavaScript, images, and additional
        resources. As a best practice, provide sufficient time (by
        waiting for the appropriate `AdEventsListener` callbacks, in
        this case, `adDidLoad()`) before calling `show()`.

    b.  When you're ready to display your ad, use
        `AdEventsListener#adDidLoad()` callback to check whether the
        returned `interstitialView` is not null.

    c.  If the returned `interstitialView` is not null, display the interstitial
        by calling the `showAsInterstitialFromRoot()` method. You can
        add logic to the other interstitial life cycle methods to handle
        what should happen when your user interacts with the ad, for
        example, resuming your game on ad dismissal through
        `adInterstitialDidClose()`.

    d.  Call `destroy()` on the interstitial in your `Activity`'s
        `onDestroy()` method.
        Your completed ad creation and load code should look something
        like this:
        
    ```java
        private void createInterstitialAD() {
         
            try {
                // Create an interstitial ad.
                interstitialView = new InterstitialView(this, "YOUR_AD_DOMAIN", "YOUR_AD_ID", InterstitialView.AdType.HTML);
            }
            catch (Exception e) {
                Log.e(TAG, "interstitialView creation failed");
            }
         
            if (interstitialView != null) {
                // Listener to get the life cycle events of an ad.
                interstitialView.addAdEventListener(this);
         
                // Sets the opacity of the ad. Recommended 1.0f.
                interstitialView.interstitialProperties.setPubBackGroundOpacity(1.0f);
         
                UserParameters userParameters = new UserParameters();
                userParameters.setUserGender(UserParameters.OXMGender.FEMALE);
                // Set extra params for data enrichment.
                interstitialView.setUserParameters(userParameters);
         
                // Set flex ad sizes.
                interstitialView.setFlexAdSize(AdConfiguration.OXMAdSize.INTERSTITIAL_320x480_300x250);
         
                // Load an ad.
                interstitialView.load();
            }
        }
     ```
     See `InterstitialActivity.java` from `AndroidSDKDemo` for a full
example.

Using AdEventsListener
--------------------------------

For interstitial ads, an events listener must always be implemented and
set before the `load()` method is called, and the
`showAsInterstitialFromRoot()` method must always be called after the
`onAdDidLoad()` event is fired. For details on available events, see
[Callbacks](android-sdk-controller-callbacks.md).
``` java
    @Override
        public void adDidLoad(final InterstitialView interstitialView, AdDetails adDetails) {
        Log.d(TAG, "Ad Loaded");
     
        btnShowInterstitial.setEnabled(true);
        btnShowInterstitial.setText("Show Interstitial");
        btnShowInterstitial.setOnClickListener(new OnClickListener() {
     
            @Override
            public void onClick(View v) {
               interstitialView.showAsInterstitialFromRoot();
             }
        });
    }
```
`AdDetails` includes a `transactionId`. The `transactionId` is a unique
identifier for an ad, which you can use for [managing and reporting ad
quality issues](android-sdk-ad-quality.md).

For information about enriching data in the ad request, see
[Parameters](android-sdk-request-params.md).

You can use `setBackgroundOpacity` to set the dim amount for the
background of the interstitial ad. The recommended setting is `1.0f`:
```java
    interstitialView.setBackgroundOpacity(1.0f);
```
