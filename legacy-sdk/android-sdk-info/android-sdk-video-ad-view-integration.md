300x250 Video integration
==============================

You can integrate 300x250 video ads in the OpenX Mobile Android SDK.
The supported video ad formats are:
- .mp4
- .3gpp 
- .webm
- .mkv.

Prerequisites
-------------------------------

Before integrating a video interstitial ad in your app, please do the
following:

1.  Either [create a linear video ad unit](https://docs.openx.com/Content/publishers/inventory-adunits-video-linear.html)
    or make sure you know which existing ad unit to use.
2.  Update your [AndroidManifest](android-sdk-integration.md).

For a complete example, see `VideoAdViewActivity.java` in the
`AndroidSDKDemo` workspace provided in the SDK zip file.

Using the Listener interface
--------------------------------------

Add an `VideoAdViewListener` and implement those callbacks to get
notifications for various events. For details, see
[Callbacks](android-sdk-controller-callbacks.md).

To listen for events, implement the listener interface.
Implement all necessary AdEventListener overrides:

```java
public class ExampleActivity extends Activity implements VideoAdViewListener
    {
        @Override
        public void adDidLoad(VideoAdView videoAdView, AdDetails adDetails) {
            Toast.makeText(getApplicationContext(), 
            "Video successfully loaded.", Toast.LENGTH_SHORT).show();
        }

        // ... implement VideoAdViewListener overrides ...
    }
        
```

and pass your `Activity` to the `VideoAdView` as it is being created:
```
videoAdView.addListener(this);
```
`AdDetails` includes a `transactionId`. The `transactionId` is a unique
identifier for an ad, which you can use for [managing and reporting ad
quality issues](android-sdk-ad-quality.md).

Load 300x250 video ads in your app
------------------------------------

1.  Define a slot for your video ad by including an XML namespace 'xmlns:app="http://schemas.android.com/apk/res-auto"' and an
    XML block in your layout XML or programmatically.

    The OpenX Mobile Android SDK provides a custom View subclass,
    `VideoAdView`, which handles requesting and loading ads.

    Option 1: Define a slot for your video ad in your layout XML.

    Start by including this XML block to your Activity's or Fragment's
    layout.

    Allowed XML attributes are:

    -   `adUnitId` = Denotes the adunit. You will create this in the
        OpenX UI.
    -   `domain` = Denotes the delivery domain on which an adUnitID was
        created (for example: `PUBLISHER-d.openx.net`).
    -   `adUnitGroupId` = Denotes the adUnitGroupId. 
    -   `autoDisplayOnLoad` = Denotes whether the ad will be displayed immediately after it's ready.

    Example:

    ```xml
    <com.openx.apollo.views.video.views.VideoAdView
        android:id="@+id/adView" 
        android:layout_width="300dp"
        android:layout_height="250dp"
        app:adUnitId="537454411"
        app:domain="PUBLISHER-d.openx.net"
        app:adUnitGroupId="1234"
        app:autoDisplayOnLoad="true"
    />
        
    ```

    Option 2: Programmatically create a videoAdView.

    Use the following constructor to create an instance of `VideoAdView` and add it to
    your container in the layout:

    ```java
    public VideoAdView(Context context, String domain, String adUnitId, String adUnitGroupId)
    ```

    For example:

    ``` java
    VideoAdView videoAdView = new VideoAdView(this, "PUBLISHER-d.openx.net", "537454411", "1234");
    // Example to add the AdView to your container:
    LinearLayout adContainer = (LinearLayout) findViewById(R.id.adContainer);
    if(videoAdView != null) {
        adContainer.addView(videoAdView);
    }
    // Where LinearLayout could be any of your layouts, like ConstraintLayout, GridLayout, RelativeLayout, and so on.
        
    ```

2.  Load an ad.

    ```java               
    private VideoAdView videoAdView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        // TODO: Initialize or set your videoAdView variable here.
        
        createAd(videoAdView);
    }
                            
    private void createAd(VideoAdView videoAdView) {
     
        if (videoAdView != null) {
     
            // Set user parameters to enrich ad request data.
            UserParameters userParameters = new UserParameters();

            userParameters.setCustomParameter("name", "OpenXDemoApp");

            // Set userparameters on videoAdView:
            videoAdView.setUserParameters(userParameters);
     
            // Set predefined flex ad size here.
            videoAdView.setFlexAdSize(AdConfiguration.OXMAdSize.INTERSTITIAL_300x250);
     
            // Auto display a video ad once loaded.
            videoAdView.setAutoDisplayOnLoad(true);
     
            // Set an ad event listener to get notified of the ad life cycle.
            videoAdView.addAdEventListener(this);
     
            // Load an ad.
            videoAdView.load();
     
            // Tip: call just videoAdView.setAutoDisplayOnLoad(true); without videoAdView.play();
            // Tip: or call videoAdView.setAutoDisplayOnLoad(false); followed by videoAdView.play();
     
        }
     
    }
    ```

3.  Remember to clean up the `VideoAdView` inside `onDestroy()`.

    ```java
    @Override
    protected void onDestroy() {
                            
        super.onDestroy();
                            
        if (videoAdView != null) {
                            
            videoAdView.destroy();
            videoAdView = null;
                            
        }
    }

Features and Behaviour
------------------------------------
300x250 Video supports all video ad features of SDK
-   Open Measurement tracking
-   AdChoices functionality 
-   End Card 
-   MRAID End Card
-   Pause and resume playback on showing clickthrough or AdChoices page
300x250 Video does not support autoplay and autostop on changing viewability, for example in the feed. This behaviour can be implemente by users of the SDK. 

For information about enriching data in the ad request, see
[Parameters](android-sdk-request-params.md).
