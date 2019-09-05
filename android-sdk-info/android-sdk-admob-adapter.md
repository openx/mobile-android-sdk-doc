AdMob SDK adapter
=================

If you want to use OpenX as a source of demand on AdMob via the Google
Mobile Ads SDK, you can build a custom event adapter to integrate the
OpenX Mobile Android SDK with the Google Mobile Ads SDK.

To help you quickly build your adapter, OpenX provides sample adapter
code for banner, interstitial display, and opt-in (rewarded) video ads
in the AdMob adapter sample app. The sample adapters are built according
to [AdMob's instructions](https://developers.google.com/admob/android/mediation/#custom_events).
Make sure you integrate the ad format into your app according to the
[Prerequisites](https://developers.google.com/admob/android/mediation/#prerequisites)
indicated by AdMob.

> **Note:** You can also build your own custom event adapter to mediate OpenX video
interstitial ads.

1.  Get the following from OpenX:
    -   OpenX Mobile Android SDK.
    -   AdMob adapter code. This code is separate from the OpenX Mobile
        SDK package.
    -   For banner and interstitial display:
        -   OpenX ad unit ID.
        -   OpenX delivery domain (for example,
            `PUBLISHER-d.openx.net`).
        -   For opt-in video, the ad unit id and domain.
2.  Make sure you have imported the OpenX Mobile Android SDK into your
    project and integrated it into your app. See:
    -   [Importing the Android SDK](android-sdk-importing.md)
    -   [Integrating the SDK with your
        project](android-sdk-integration.md)
3.  Add the Google Mobile Ads SDK to your project according to [AdMob\'s
    instructions](https://developers.google.com/admob/android/quick-start).
4.  Add the OpenX AdMob adapters to your project where needed for your
    app (in the samples from OpenX, you will see them in `com.admob`):
    -   `com.admob.AdmobBannerAdapter`

    -   `com.admob.AdmobInterstitialAdapter`

    -   `com.admob.AdmobRewardedVideoAdapter`
    
    -   `com.admob.AdmobVideoInterstitialAdapter`

5.  In the AdMob UI, choose the mediation group to which you will add
    OpenX. For example, if you want to mediate OpenX rewarded video ads,
    choose an Android rewarded video mediation group that you have
    created.
6.  In the AdMob UI, in the mediation group, define a custom event. Use
    the following settings:
    -   **Label**: a label of your choice to describe this OpenX custom
        event.
    -   **eCPM**: an estimate of the revenue you receive for every
        thousand ad impressions.
    -   **Class Name**: the class name of your custom event adapter.
    -   **Parameter**: the OpenX ad unit ID and delivery domain.
    Please use the following format:
    
-   For banner and interstitial ads:

    ```
    {
    "AD_UNIT_ID": "[OPENX AD UNIT]", 
    "AD_DOMAIN": "[OPENX DELIVERY DOMAIN]"
    }
    ```

    For example:

    ``` 
    {
    "AD_UNIT_ID": "123456789", 
    "AD_DOMAIN": "ox-d.mobile.servedbyopenx.com"
    }
    ```

-   For video interstitial and opt-in video ads:

    ``` 
    {
    "AD_UNIT_ID": "[OPENX AD UNIT]", 
    "AD_DOMAIN": "[OPENX DELIVERY DOMAIN]"
    }
    ```

    Or with Ad Group Unit ID

    ```
    {
    "AD_UNIT_ID": "[OPENX AD UNIT]", 
    "AD_DOMAIN": "[OPENX DELIVERY DOMAIN]",
    "AD_UNIT_GROUP_ID": "[AD UNIT GROUP ID]"
    }
    ```

    For example:

    ``` 
    {
    "AD_UNIT_ID": "123456789", 
    "AD_DOMAIN": "ox-d.mobile.servedbyopenx.com",
    "AD_UNIT_GROUP_ID": "123456"
    }
    ```
    
    ```
    {
    "AD_DOMAIN": "ox-d.mobile.servedbyopenx.com", 
    "AD_UNIT_GROUP_ID": "123456789",
    }
    ``` 

7.  Save the custom event and the mediation group.

8.  Create a separate custom event for each ad format that you decide to
    use (banner, interstitial display, and rewarded video).
9.  [Test](android-sdk-self-test.md) your implementation and notify
    OpenX to enable live traffic.
