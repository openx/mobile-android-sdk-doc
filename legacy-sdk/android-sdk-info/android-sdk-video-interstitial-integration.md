Video interstitial integration
==============================

You can integrate video interstitial ads in the OpenX Mobile Android
SDK. The supported video ad formats are:
- .mp4
- .3gpp 
- .webm
- .mkv.

Overview
--------

[Prerequisites](#prerequisites)

[Integration](#integration)

[Data enrichment](#data-enrichment)

Prerequisites
-------------------------------

Before integrating a video interstitial ad in your app, please do the
following:

1.  Either [create a linear video ad unit](https://docs.openx.com/Content/publishers/inventory-adunits-video-linear.html)
    or make sure you know which existing ad unit to use.
2.  Update your [AndroidManifest](android-sdk-integration.md).

Integration
---------------------------

1. To create a video interstitial ad, create a new object of the type
`InterstitialView` as shown below.

    ``` java
    public class MainActivity extends Activity {
 
    private InterstitialView mInterstitialView;
 
    @Override   
    protected void onCreate(Bundle savedInstanceState) {
        ...
 
        loadAd();
    }
  
    private void loadAd() {
 
        try {
            mInterstitialView = new InterstitialView(this, "mobile-d.openx.net", "123456789", InterstitialView.AdType.VAST);
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
    ``` java
    mInterstitialView.interstitialDisplayProperties.setPubBackGroundOpacity(1.0f);
    ```
3. Use `InterstitialVideoProperties` to set video properties.

    a.  Create an `InterstitialVideoProperties` object:
    ```java
        InterstitialVideoProperties properties = new InterstitialVideoProperties();
    ```
    b.  For a better user experience, preload the video as shown below. By
    default, `preLoadVideo` is set to `true`, but if you still prefer to
    stream the video, set `preLoadVideo `to `false`. Note that the SDK
    will not preload video ads larger than 25 MiB.
    ``` java
       properties.preloadVideo = true;
    ``` 
    c.  Use `autoPreloadConfigs` to specify the network scenario where the
    video is to be preloaded. The default is Wi-Fi only.
    ``` java
       properties.autoPreloadConfigs = VideoProperties.AutoVideoPreloadConfigs.WifiOnlyAutoPreload;
    ```
    d.  Use `closeButtonDelayinSecs` to display the Close button on the
    video ad at the desired interval.

    -   Videos that span 2 seconds or less cannot be closed early.
    -   Videos longer than 2 seconds will display a Close button at 2
        seconds by default (recommended).

    If you prefer customizing the video duration, set the limits as
    described below. All values are in seconds.

    > **Note:** The upper limit is the end of video or 30 seconds, whichever is
    less.
    
    | Video ad duration | Custom value                    | Result       |
    | ----------------- | ------------------------------- | ------------ |
    | &lt;= 2           | Ignored                         | End of video |
    | &gt; 2            | Nil, value &lt; 2               | 2            |
    | &gt; 2            | 2 &lt;= value &lt;= upper limit | Value        |
    | &gt; 2            | Value &gt; upper limit          | Upper limit  |

     

    e.  After you have set the video properties, attach them to `InterstitialView.`

    ``` java
    mInterstitialView.setInterstitialVideoProperties(properties);
    ```

4. Create and attach `AdEventsListener` to `InterstitialView` to get the life cycle
events of the ad. For details on available events, see
[Callbacks](android-sdk-controller-callbacks.md).

    ``` java
    mInterstitialView.addAdEventListener(new AdEventsListener() {
     
        @Override
        public void adDidLoad(InterstitialView interstitialView, AdDetails adDetails) {
 
        }
 
    ...
    };
    ```

5. Load the ad.

    ``` java
    mInterstitialView.load();
    ```

6. At any point after the ad is loaded, which is indicated by the
`adDidLoad()` event being called inside your `AdEventsListener`, you can
display the ad.

    ``` java
    mInterstitialView.showVideoAsInterstitial();
    ```

7. Be sure to destroy the `InterstitialView` object during the cleanup.

    ``` java
    @Override
    protected void onDestroy() {
        ...
    
       if (null != mInterstitialView) {
          mInterstitialView.destroy();
          mInterstitialView = null;
       }
    }
    ```

Complete example
----------------

``` java
public class MainActivity extends Activity {
 
    // InterstitialView that displays the video ad
    private InterstitialView mInterstitialView;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        ...
 
        // Load ad
        loadAd();
    }
 
    private void loadAd() {
 
        // If an InterstitialView already exists, re-use it
        if (mInterstitialView != null) {
            mInterstitialView.load();
            return;
        }
  
        try {
            // Initialize InterstitialView
            mInterstitialView = new InterstitialView(this, "mobile-d.openx.net", "123456789", InterstitialView.AdType.VAST);
        }
        catch (AdException e) {
            Log.e(TAG, "Something went wrong! Error: " + e.getMessage());
            return;
        }
     
        // Set the opacity of the ad. Recommended 1.0f.
        mInterstitialView.interstitialDisplayProperties.setPubBackGroundOpacity(1.0f);
     
        // Create video properties object
        InterstitialVideoProperties properties = new InterstitialVideoProperties();
     
        // Enable preloading of the video
        properties.preloadVideo = true;
     
        // Attach the video properties to the InterstitialView
        mInterstitialView.setInterstitialVideoProperties(properties);
     
        // Create and attach an AdEventsListener to the InterstitialView
        mInterstitialView.addAdEventListener(new AdEventsListener() {
     
            @Override
            public void adDidLoad(InterstitialView interstitialView, AdDetails adDetails) {
                // After ad is loaded, show the video
                mInterstitialView.showVideoAsInterstitial();
            }
                                
            ...
        });
 
        // Load the ad
        mInterstitialView.load();
    }   
 
    @Override
    protected void onDestroy() {
        ...
 
        // Destroy the InterstitialView
        if (mInterstitialView != null) {
            mInterstitialView.destroy();
            mInterstitialView = null;
        }
    }
}
```

Data enrichment
------------------------

For information about enriching data in the ad request, see [Parameters](android-sdk-request-params.md).
