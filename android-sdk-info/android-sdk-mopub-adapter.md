MoPub SDK adapter
=================

If you want to use OpenX as a source of demand on MoPub via the MoPub
SDK, you can build an adapter to integrate the OpenX Mobile Android SDK
with the MoPub SDK.

To help you quickly build your adapter, OpenX provides sample adapter
code for banner, interstitial, video interstitial, and opt-in (rewarded)
video ads in the MoPub adapter sample app included in the [OpenX Mobile
Android SDK](android-sdk-getting-started.md#sdk-zip-file-contents). The sample adapters
are built according to [MoPub's
instructions](https://www.mopub.com/resources/docs/mopub-network-mediation/writing-custom-events-for-non-supported-networks-android/).
You will need to customize each adapter to meet your needs as instructed
below.

1. Add the MoPub SDK as a dependency.

```
[build.gradle (Module: app)]
dependencies {
...
    compile('com.mopub:mopub-sdk:x.y.z@aar') {
        transitive = true
    }
}
```

2. Copy and paste the adapter files from the **Mediation Adapters** folder
in the OpenX Mobile Android SDK zip file into your project. Place the
files in this location:

`com.mopub.mobileads`

3. In the MoPub web interface, in an order, create a network line item
using the **Custom Native Network** type. Place the fully-qualified
class name of your custom event (for example,
`com.mopub.mobileads.MopubBannerAdapter`) in the **Custom Class** field.

4. In the **Data** field, use the following formats:

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
    "AD_UNIT_ID": "123456789", 
    "AD_DOMAIN": "ox-d.mobile.servedbyopenx.com"
    }
    ```

5. Select the app and ad unit for the line item to target.

6. If using multiple adapters (`MopubBannerAdapter`,
`MopubInterstitialAdapter`, and/or
`MopubRewardedVideoInterstitialAdapter`), create a separate line item
for each adapter.

7. [Test](android-sdk-self-test.md) your implementation and notify OpenX
to enable live traffic.
