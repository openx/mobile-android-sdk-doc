Banner integration
==================

Prerequisites
------------------------

Before integrating a banner ad in your app, please do the following:

1.  Either  [create a mobile ad
    unit](https://docs.openx.com/Content/publishers/inventory_createmobilead.html)
    or make sure you know which existing ad unit to use.
2.  Update your [AndroidManifest](android-sdk-integration.md).

For a complete example, see `BannerActivity.java` in the
`AndroidSDKDemo` workspace provided in the SDK zip file.

Using the Listener interface
--------------------------------------

Add an `AdEventsListener` and implement those callbacks to get
notifications for various events. For details, see
[Callbacks](android-sdk-controller-callbacks.md).

To listen for events, implement the listener interface.
Implement all necessary AdEventListener overrides:

```java
public class ExampleActivity extends Activity implements AdEventsListener
    {
        @Override
        public void adDidLoad(BannerView bannerView, AdDetails adDetails) {
            Toast.makeText(getApplicationContext(),
            "Banner successfully loaded.", Toast.LENGTH_SHORT).show();
        }

        // ... implement AdEventsListener overrides ...
    }

```

and pass your `Activity` to the `BannerView` as it is being created:
```
bannerView.addAdEventListener(this);
```
`AdDetails` includes a `transactionId`. The `transactionId` is a unique
identifier for an ad, which you can use for [managing and reporting ad
quality issues](android-sdk-ad-quality.md).

Load banner ads in your app
------------------------------------

1.  Define a slot for your banner ad. You can do this by including an
    XML block in your layout XML or programmatically.

    The OpenX Mobile Android SDK provides a custom View subclass,
    `BannerView`, which handles requesting and loading ads.

    Option 1: Define a slot for your banner ad in your layout XML.

    First declare XML namespace on top of your layout: `xmlns:app="http://schemas.android.com/apk/res-auto"`.
    And then include this XML block to your Activity's or Fragment's
    layout.

    Allowed XML attributes are:

    -   `domain` = Denotes the delivery domain on which an adUnitID was
        created (for example: `PUBLISHER-d.openx.net`).
    -   `adUnitId` = Denotes the adunit. You will create this in the
        OpenX UI.
    -   `autoRefreshDelay` = Time in seconds at which the ad needs to be
        refreshed.
    -   `autoRefreshMax` = Number of times the ad can be refreshed once
        it is displayed. By default, there is no maximum, and the ad
        will continue to refresh until the `AdView` is destroyed.
    -   `flexAdSize` = Denotes the ad sizes allowed for this ad unit.
        See [Flex ads.](android-sdk-flex-ads.md).

    Example:

    ```xml
    <com.openx.view.plugplay.views.banner.BannerView
        android:id="@+id/adView"
        android:layout_width="320dp"
        android:layout_height="50dp"
        app:domain="PUBLISHER-d.openx.net"
        app:adUnitId="537454411"
        app:autoRefreshDelay="30"
        app:autoRefreshMax="50"
        app:flexAdSize = "320x50"
    />

    ```

    Option 2: Programmatically create a banner.

    Use the following constructor to create an instance of `BannerView` and add it to
    your container in the layout:

    ```java
    public BannerView(Context context, String domain, String adUnitId)
    ```

    For example:

    ``` java
    BannerView bannerView = new BannerView(this, "PUBLISHER-d.openx.net", "537454411");
    // Example to add the AdView to your container:
    LinearLayout adContainer = (LinearLayout) findViewById(R.id.adContainer);
    if(bannerView != null) {
        adContainer.addView(bannerView);
    }
    // Where LinearLayout could be any of your layouts, like ConstraintLayout, GridLayout, RelativeLayout, and so on.

    ```

2.  Load an ad.

    ```java               
    private BannerView bannerView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // TODO: Initialize or set your bannerView variable here.

        createAd(bannerView);
    }

    private void createAd(BannerView bannerView) {

        if (bannerView != null) {

            // Set user parameters to enrich ad request data.
            UserParameters userParameters = new UserParameters();

            userParameters.setCustomParameter("name", "OpenXDemoApp");

            // Set userparameters on bannerView:
            bannerView.setUserParameters(userParameters);

            // Set predefined flex ad size here.
            bannerView.setFlexAdSize(AdConfiguration.OXMAdSize.BANNER_320x50);

            // Or, set your custom flex ad size as a string.
            // bannerView.setFlexAdSize("320x50,400x350");

            // Set an interval at which this banner should refresh.
            bannerView.setAutoRefreshDelay(30);

            // Set the maximum number of times the banner should refresh.
            bannerView.setAutoRefreshMax(3);

            // Set an ad event listener to get notified of the ad life cycle.
            bannerView.addAdEventListener(this);

            // Load an ad.
            bannerView.load();

            // Tip: call just bannerView.setAutoDisplayOnLoad(true); without bannerView.show();
            // Tip: or call bannerView.setAutoDisplayOnLoad(false); followed by bannerView.show();

        }

    }
    ```

3.  Remember to clean up the `BannerView` inside `onDestroy()`.

    ```java
    @Override
    protected void onDestroy() {

        super.onDestroy();

        if (bannerView != null) {

            bannerView.destroy();
            bannerView = null;

        }
    }

    ```

For information about enriching data in the ad request, see [Parameters](android-sdk-request-params.md).

Refreshing ads
---------------------------

The `BannerView` will automatically refresh your ad unit at a [time interval
that you set](https://docs.openx.com/Content/publishers/inventory_auto-refresh.html) using the
OpenX user interface.

You can also programmatically change the settings for auto-refresh on a
particular `BannerView` using `setAutoRefreshDelay()` and
`setAutoRefreshMax()` methods.

-   `setAutoRefreshDelay()` = Interval at which an ad has to be
    refreshed.
-   `setAutoRefreshMax()`= Number of times an ad can be refreshed, if
    you want to set a maximum. By default, there is no maximum.

``` java
//Time at which displayed ad has to be refreshed.
bannerView.setAutoRefreshDelay(30);
bannerView.setAutoRefreshMax(50);
```                
OR
```xml
<com.openx.view.plugplay.views.banner.BannerView
...
app:autoRefreshDelay="30"
app:autoRefreshMax="50"
... />
```

Notes:

-   Server-side refresh values will take precedence over client-side
    refresh values.
-   The allowed range of the refresh interval is 30 to 125 seconds.
-   If you do not set any refresh interval, the ad will refresh at a
    default interval of 60 seconds.
-   If you do not want an ad to be refreshed, please set the interval to
    `0` in `bannerView.setAutoRefreshDelay(0);`.

It is recommended to have one of the below for the right behavior. Note
that this applies only when using the OpenX SDK without any
[adapter](android-sdk-mopub-adapter.md):

Example:

``` java
bannerView.setAutoRefreshDelay(your_refresh_value_in_secs);
```

Custom refresh delay
------------------------

You can disable the refresh on an ad unit by setting `autoRefreshDelay` to \"`0`\". Or, use
`setAutoRefreshDelay(0)`.

Example:

``` xml
<com.openx.view.plugplay.views.banner.BannerView
    android:id="@+id/adBanner_layout_manual"
    app:adUnitId="537454411"
    app:autoRefreshDelay="0"
    app:domain="PUBLISHER-d.openx.net"
    app:flexAdSize = "320x50"
    android:layout_width="320dp"
    android:layout_height="50dp"
    android:layout_alignParentTop="true"
    android:layout_centerHorizontal="true" />
```

`AdEventsListener` provides `adDidLoad()` when the ad is loaded.
