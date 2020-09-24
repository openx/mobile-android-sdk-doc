Opt-in video integration
========================

The supported video ad formats are:
- .mp4
- .3gpp 
- .webm
- .mkv.

Prerequisites
-------------------------------

Before integrating an opt-in video (rewarded video) ad in your app,
please do the following:

1.  Either [create
    a linear video ad
    unit](https://docs.openx.com/Content/publishers/inventory-adunits-video-linear.html)
    with the **Is Rewarded** check box selected or make sure you know
    which existing ad unit to use.
2.  Update your [AndroidManifest](android-sdk-integration.md).

Integration
---------------------------

1. To create an opt-in video ad, create a new object of the type
`InterstitialView` as shown below.

    ``` java
    public class MainActivity extends Activity {
    
    private InterstitialView mAdViewVideo;
     
     @Override   
     protected void onCreate(Bundle savedInstanceState) {
         ...
     
            loadAd();
        }
  
       private void loadAd() {
     
         try {
             mAdViewVideo = new InterstitialView(this, "mobile-d.openx.net", "539733507", InterstitialView.AdType.VAST);
         }
         catch (AdException e) {
             Log.e(TAG, "Something went wrong! Error: " + e.getMessage());
             return;
         }
                     
         ...
     }
    }
    ```

2. Set the opacity of the video frame. The recommended value is `1.0f`
(opaque).
    ```java
    mAdViewVideo.interstitialDisplayProperties.setPubBackGroundOpacity(1.0f);
    ```
3. Use `InterstitialVideoProperties` to set video properties.

    a. Create an `InterstitialVideoProperties` object.
    ```java
       InterstitialVideoProperties properties = new InterstitialVideoProperties();
    ```
    b.  For a better user experience, preload the video as shown below. Note
    that the SDK will not preload video ads larger than 25 MiB.
    ```java
        properties.preloadVideo = true;
    ```
    c.  Use `autoPreloadConfigs` to specify the network scenario where the
    video is to be preloaded. The default is Wi-Fi only.
    ```java
        properties.autoPreloadConfigs = VideoProperties.AutoVideoPreloadConfigs.WifiOnlyAutoPreload;
    ```
    d.  After you have set the video properties, attach them to `InterstitialView`.

    ``` java
    mAdViewVideo.setInterstitialVideoProperties(properties);
    ```

4. Create and attach `AdEventsListener` to `InterstitialView` to get the life cycle
events of the ad. For details on available events, see
[Callbacks](android-sdk-controller-callbacks.md).

    ``` java
    mAdViewVideo.addAdEventListener(new AdEventsListener() {
     
        @Override
        public void adDidLoad(InterstitialView adView, AdDetails adDetails) {
    
        }
 
     ...
    };
    ```

5. Set the ad unit as an opt-in video.

    ``` java
    mAdViewVideo.setRewardedFlag(true);
    ```

6. Load the ad.

    ``` java
    mAdViewVideo.load();
    ```

7. At any point after the ad is loaded, which is indicated by the
`adDidLoad()` event being called inside your `AdEventsListener`, you can
display the ad.

    ``` java
    mAdViewVideo.showVideoAsInterstitial();
    ```

8. Be sure to destroy the `InterstitialView` object during the cleanup.

    ``` java
    @Override
    protected void onDestroy() {
        ...

        if (null != adViewVideo) {
            mAdViewVideo.destroy();
            mAdViewVideo = null;
        }
    }
    ```

Complete example
----------------

``` java
public class MainActivity extends Activity {
 
    // InterstitialView that displays the video ad
    private InterstitialView mAdViewVideo;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        ...
 
        // Load ad
        loadAd();
    }
 
    private void loadAd() {
 
        // If an InterstitialView already exists, re-use it
        if (mAdViewVideo != null) {
            mAdViewVideo.load();
            return;
        }
  
        try {
            // Initialize InterstitialView
            mAdViewVideo = new InterstitialView(this, "mobile-d.openx.net", "539733507", InterstitialView.AdType.VAST);
        }
        catch (AdException e) {
            Log.e(TAG, "Something went wrong! Error: " + e.getMessage());
            return;
        }
     
        // Disable auto-display for interstitial video
        mAdViewVideo.setAutoDisplayOnLoad(false);
     
        // Set the opacity of the ad. Recommended 1.0f.
        mAdViewVideo.interstitialDisplayProperties.setPubBackGroundOpacity(1.0f);
     
        // Create video properties object
        InterstitialVideoProperties properties = new InterstitialVideoProperties();
     
        // Enable preloading of the video
        properties.preloadVideo = true;
     
        // Attach the video properties to the InterstitialView
        mAdViewVideo.setInterstitialVideoProperties(properties);
     
        // Create and attach an AdEventsListener to the InterstitialView
        mAdViewVideo.addAdEventListener(new AdEventsListener() {
     
            @Override
            public void adDidLoad(AdView adView, AdDetails adDetails) {
                // After ad is loaded, show the video
                mAdViewVideo.showVideoAsInterstitial();
            }
                                
            ...                         
        });

        // Set the ad as opt-in video                        
        mAdViewVideo.setRewardedFlag(true);
                                
        // Load the ad
        mAdViewVideo.load();
    }   
 
    @Override
    protected void onDestroy() {
        ...
 
        // Destroy the InterstitialView
        if (mAdViewVideo != null) {
            mAdViewVideo.destroy();
            mAdViewVideo = null;
        }
    }
}
```

For information about enriching data in the ad request, see [Parameters](android-sdk-request-params.md).
