Parameters
===================

The tables below list the methods and properties that the OpenX Mobile Android SDK allows to customize. 
The more actual info about the user, the app, and the device you provide the more chances to win an impression. 
Please strictly follow the recommendations in the below tables and provide all ❗ **Required** and **Highly Recommended** values.


1. [UserParameters](#userparameters)
2. [OXSettings](#oxsettings)
3. [BannerView](#bannerview)
4. [InterstitialView](#interstitialview)
5. [InterstitialDisplayProperties](#interstitialdisplayproperties)

UserParameters
------------------------------------------------------
| **Parameter**              | **Method**                | Description                                                  | Required?|
| -------------------------- | ------------------------- | ------------------------------------------------------------ | -------- |
| age                        | `setUserAge`              | Age of the user in years. For example: `35`   | ❗ Highly Recommended  |
| buyerid                    | `setBuyerId`              | Buyer-specific ID for the user as mapped by the exchange for the buyer. | Optional |
| crr                        | `setCarrier`              |  Mobile carrier - Defined by the Mobile Country Code (MCC) and Mobile Network Code (MNC), using the format: <MCC>-<MNC>. For example: `"310-410"` | Optional |
| customdata                 | `setUserCustomData`       | Optional feature to pass bidder data that was set in the exchange’s cookie. The string must be in base85 cookie safe characters and be in any format. Proper JSON encoding must be used to include “escaped” quotation marks. | Optional |
| dma                        | `setDma`                  | A designated market are. For US locations, indicates the end-user's Designated Market Area. For example: dma=803. | Optional |
| eth                        | `setUserEthnicity`        | Ethnicity of the user (African American, Asian, Hispanic, White, Other). For example: `OXMEthnicity.ASIAN` | Recommended if available |
| ext                        | `setUserExt`              | Placeholder for exchange-specific extensions to OpenRTB. | Optional |
| gen                        | `setUserGender`           | The gender of the user (Male, Female, Other, Unknown). For example: `OXMGender.FEMALE`  | ❗ Highly Recommended |
| inc                        | `setUserAnnualIncomeInUs` | Annual income of the user in US dollars. For example: `55000`| ❗ Highly Recommended  |
| ip                         | `setIpAddress`            | The IP address of the carrier gateway. If this is not present, OpenX retrieves it from the request header. For example: `"192.168.0.1"` | ❗ Highly Recommended |
| keywords                   | `setUserKeywords`         | Comma separated list of keywords, interests, or intent. | Optional |
| lat, lon                   | `setUserLatLng`           | Location of the user’s home base defined by a provided longitude and latitude. It's highly recommended to provide Geo data to improve the request.| Optional |
| mar                        | `setUserMaritalStatus`    | The marital status of the user (Single, Married, Divorced, Unknown). For example: `OXMMaritalStatus.DIVORCED` | Recommended if available  |
| publisher                  | `setPublisherName`        | Publisher name (may be aliased at the publisher’s request).| Recommended if available  |
| url/storeurl               | `setAppStoreMarketUrl`    | The URL for the mobile application in Google Play. That field is required in the request. <br />**For example:**` https://play.google.com/store/apps/details?id=com.outfit7.talkingtom`. | ❗ Required  |
| xid                        | `setUserId`               | ID of the user within the app. For example: `"24601"` | ❗ Highly Recommended  |

### How to set user parameters

You can use `UserParameters` to pass ad call request parameters.

You can manually set custom parameters using
`setUserParameters()` on `AdView` and send it through the
corresponding `load()`.

``` java
// Set user parameters to enrich ad request data.
// Please see UserParameters for the userKeys and the APIs available.
UserParameters userParameters = new UserParameters();
userParameters.setUserKeywords("socialNetworking");
userParameters.setUserAge(18);
userParameters.setUserAnnualIncomeInUs(50000);
 
// Set parameters.
// userParameters.setCustomParameters(Hashtable<String, String> params)
// userParameters.setParameters(Hashtable<String, String> params)
// clear parameters
// userParameters.clearParameters()
// userParameters.clearParameter(String key)
 
adView.setUserParameters(userParameters);
```

### Custom key-value parameters

You can submit values through `UserParameters` for the extended (`c.xxx`) ad-call
parameters.

| **Parameter**           | **Method**          | **Description**                                              |
| ----------------------- | ------------------- | ------------------------------------------------------------ |
| custom<br />parameter   | setCustomParameter  | A custom user parameter auto-prepended with c..<br />You should provide the plain name of the parameter, such as xxx, which will be changed to c.xxx when sent. |
| custom <br />parameters | setCustomParameters | Custom user parameters, which consist of a dictionary of name-value parameter pairs, where each param name will be automatically prepended with c.. |

OXSettings
-------------------------------------------

  | **Field**               | **Description**                                              | **Default** |
  | ----------------------- | ------------------------------------------------------------ | ----------- |
  | defaultDomain           | Controls the initial value of `domain` for all newly created `BannerView, InterstitialView and VideoAdView`. Useful if the same domain is in use throughout your app. | Null        |
  | defaultAdUnitId         | Controls the initial value of `adUnitID` for all newly created `BannerView, InterstitialView and VideoAdView`. Useful if the same domain is in use throughout your app. | Null        |
  | defaultAutoRefreshDelay | Controls the initial value of `autoRefreshDelay` for all newly created OXMAdViews in seconds. | 60          |
  | logLevel                | Controls the type of messages of the internal logger. Options are:<br />- DEBUG - this is the noisiest level.<br />- ERROR<br />- WARN<br />- NONE | NONE        |
  | sendMRAIDSupportParams  | If `true`, the SDK sends "`af=3,5`", indicating support for MRAID. | true        |

The code sample:

``` java
OXSettings.defailtDomain = "mobile-d.openx.net";
OXSettings.defaultAutoRefreshDelay = 30;
```

BannerView
--------------------------

| **Property**               | **Method**                                                     | **Description**                                              | **Required?**                                                |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| adUnitID                   | View constructor                                                    | OpenX ad unit ID. For example: `"123456789"`            | Required |
| domain                     | View constructor                                                    | Your app's OpenX delivery domain, provided to you by OpenX. For example: `"PUBLISHER-d.openx.net"` | Required |
| adEventListener            | addAdEventListener | [Event listener](android-sdk-controller-callbacks.md) for the `BannerView`. Typically a `BannerViewListener`. | Recommended                                                |
| userParameters             | setUserParameters | Data structure containing [properties](android-sdk-request-params.md#userparameters) for enriching requests with user data. | Recommended                                                  |
| flexAdSize                 | setFlexAdSize                                                    | Allows multiple ad sizes for an ad unit (flex ads) which allows for better monetization. See [Flex ads](android-sdk-flex-ads.md). | Recommended                     |
| autoRefreshDelay           | setAutoRefreshDelay                                               | Amount of time in seconds between refreshes. This value will be overwritten with any values received from the server. Prevent an auto-refresh by using a value of `0` or less. | Optional   <br />Default is `60`                                                  |
| autoRefreshMax             | setAutoRefreshMax                                                  | Maximum number of times the `BannerView`should refresh. This value will be overwritten with any values received from the server. Using a value of 0 indicates there is no maximum. | Optional                                                      <br />Default is `0`|

The code sample:
```java
BannerView bannerView = new BannerView(this, "PUBLISHER-d.openx.net", "537454411");

UserParameters userParameters = new UserParameters();
userParameters.setCustomParameter("name", "OpenXDemoApp");
bannerView.setUserParameters(userParameters);

bannerView.setFlexAdSize(AdConfiguration.OXMAdSize.BANNER_320x50);

bannerView.setAutoRefreshDelay(30);
bannerView.setAutoRefreshMax(3);

bannerView.addAdEventListener(*Your BannerViewListener implementation*);
```

InterstitialView
-------------------------------------------------------

| **Property**                  | **Method**                                                     | **Description**                                              | **Required?**                                                |
| ----------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| adUnitIdentifierType          | View constructor                                         | Type of ad unit.<br />Options include:<br />- `AdUnitIdentifierTypeAuid`. Use for loading display ads.<br />- `AdUnitIdentifierTypeVast`. Use for loading a video ads.<br />The default is `AdUnitIdentifierTypeAuid`. | Required                                                     |
| adUnitID                      | View constructor                                                    | OpenX ad unit ID. For example: `"123456789"`            | Required |
| domain                        | View constructor                                                     | Your app's OpenX delivery domain, provided to you by OpenX. For example: `"PUBLISHER-d.openx.net"` | Required  |
| delegate                      | addAdEventListener | [Event listener](android-sdk-controller-callbacks.md) for the `InterstitialView`. Typically a `InterstitialViewListener`. | Recommended                                                 |
| interstitialDisplayProperties | [interstitialDisplayProperties](#interstitialdisplayproperties) | Data structure containing [properties](#interstitialdisplayproperties) for displaying interstitials. | Recommended                      |
| interstitialVideoProperties   | setInterstitialVideoProperties | Data structure containing properties for displaying video interstitials. | Recommended                       |
| userParameters                | setUserParameters | Data structure containing [properties](#userparameters) for enriching requests with user data. | Required                                                  |
| flexAdSize                    | setFlexAdSize                                                     | Allows multiple ad sizes for an ad unit (flex ads) which allows for better monetization. See [Flex ads](android-sdk-flex-ads.md). | Recommended                      |
| isRewarded                    | setRewardedFlag                                                        | Sets a video interstitial ad unit as an opt-in video. | Required for [opt-in interstitial video](android-sdk-video-optin-integration.md) (rewarded video) ad units |

The code sample:
```java
     
                // Create an interstitial ad.
                interstitialView = new InterstitialView(this, "YOUR_AD_DOMAIN", "YOUR_AD_ID", InterstitialView.AdType.HTML);
            
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
```

InterstitialDisplayProperties
---------------------------------------------------------------------

| **Property**        | **Method**                    | **Description**                                              |
| ------------------- | --------------------------- | ------------------------------------------------------------ |
| background opacity  | setPubBackGroundOpacity                     | Sets interstitial background opacity |

The code sample: 
```
mInterstitialView.interstitialDisplayProperties.setPubBackGroundOpacity(1.0f);
```