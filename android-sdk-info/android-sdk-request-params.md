Parameters
===================

OpenX Mobile Android SDK provides the following types of parameters:

-   [Automatically-detected parameters](#automatically-detected-parameters)
-   [Developer-provided user parameters](#developer-provided-user-parameters)
-   [Configuration parameters](#configuration-parameters)
-   [Custom key-value parameters](#custom-key-value-parameters)

Automatically-detected parameters
----------------------------------------------

The OpenX Mobile Android SDK automatically detects and determines
certain parameters.

> **Note:** OpenX does not use publisher-provided values unless the SDK fails to
auto-detect values. This is true for everything but location values.

See also [OpenRTB fields set by OpenX](https://docs.openx.com/Content/developers/ad_request_api/openrtb_fields_not_passed.html).


| **Parameter** | **Method**  | Description                                                  |
| ------------- | ----------- | ------------------------------------------------------------ |
| af            | None        | The comma-delimited list of API frameworks supported on the user's device, which can be one or more of the following:<br />-   `2` (VPAID 2.0)<br />-   `3` (MRAID 1.0)<br />-   `4` (ORMMA)<br />-   `5` (MRAID 2.0)<br />-   `6` (MRAID 3.0)<br />-   `7` (OMID-1)<br /><br />**For  example:**  `af=3,5,1 `<br /><br />**Tip:** If you want to opt out of MRAID ads from buyers, set `sendMRAIDSupportParams=false`. Note that this setting will be overridden by any [API framework setting in the ad unit](https://docs.openx.com/Content/publishers/inventory_createmobilead.html). |
| sp            | None        | OpenX Mobile SDK platform type (iOS or Android) .            |
| stt           | `setState`  | The U.S. state where the user is located.<br />**For example:** california |
| sv            | None        | OpenX Mobile Android SDK version, such as 4.0.               |
| xid           | `setUserId` | The customer-provided user ID if different from the device ID (did).<br />**For example:** 234hskhjfdkhjs |

Developer-provided parameters
------------------------------------------

Depending on the app content, you can set the following parameters for
the app user. Please check UserParameters.java for the available APIs.

> **Note:** Some of the user parameters have been deprecated. For examples of how to
use the new parameters, see [how to set user parameters](#how-to-set-user-parameters) after the
table.

| **Parameter**              | **Method**                | Description                                                  |
| -------------------------- | ------------------------- | ------------------------------------------------------------ |
| additional OpenX parameter | `setParameter`            | If an ad call parameter exists in the OpenX Ad Server but does not exist in OpenX Mobile Android SDK, use this method to add the new parameter by name and set the necessary value. |
| `age`                      | `setUserAge`              | This parameter is deprecated. Use:<br />`setParameter(UserParameters.KEY_AGE, age)` |
| custom parameters          | `setParameters`           | If multiple ad call parameters exist in the OpenX ad server but do not exist in OpenX Mobile Android SDK, use this method to add the set of key-value pairs for the new parameters. |
| dma                        | `setDma`                  | This parameter is deprecated. Use:<br />`setParameter(UserParameters.KEY_MARKET_AREA, dma)` |
| eth                        | `setUserEthnicity`        | This parameter is deprecated. Use:<br />`setParameter(UserParameters.KEY_ETHNICITY, eth)` |
| gen                        | `setUserGender`           | This parameter is deprecated.                                |
| inc                        | `setUserAnnualIncomeInUs` | This parameter is deprecated. Use:<br />`userParameters.setParameter(UserParameters.KEY_INCOME, inc);` |
| mar                        | `setUserMaritalStatus`    | This parameter is deprecated. Use:<br />`userParameters.setParameter(UserParameters.KEY_MARITAL_STATUS, UserParameters.MARITAL_SINGLE);` |
| url                        | `setAppStoreMarketUrl`    | The URL for the mobile application in Google Play.<br />**For example:**` https://play.google.com/store/apps/details?id=com.outfit7.talkingtom` |

### How to set user parameters

You can use `UserParameters` to pass ad call request parameters.

You can manually set custom parameters using
`setUserParameters()` on `AdView` and send it through the
corresponding `load()`.

``` java
// Set user parameters to enrich ad request data.
// Please see UserParameters for the userKeys and the APIs available.
UserParameters userParameters = new UserParameters();
userParameters.setCustomParameter("keywords", "socialNetworking");
userParameters.setParameter(UserParameters.KEY_AGE, "18");
userParameters.setParameter(UserParameters.KEY_INCOME, "50000");
 
// Set parameters.
// userParameters.setCustomParameters(Hashtable<String, String> params)
// userParameters.setParameters(Hashtable<String, String> params)
// clear parameters
// userParameters.clearParameters()
// userParameters.clearParameter(String key)
 
adView.setUserParameters(userParameters);
```

Configuration parameter - OXSettings
--------------------------------------------------

  | **Field**               | **Description**                                              | **Default** |
  | ----------------------- | ------------------------------------------------------------ | ----------- |
  | defaultDomain           | Controls the initial value of `domain` for all newly created `AdViews`. Useful if the same domain is in use throughout your app. | Null        |
  | defaultAdUnitId         | Controls the initial value of `adUnitID` for all newly created `AdViews`. Useful if the same domain is in use throughout your app. | Null        |
  | defaultAutoRefreshDelay | Controls the initial value of `autoRefreshDelay` for all newly created OXMAdViews in seconds. | 60          |
  | logLevel                | Controls the type of messages of the internal logger. Options are:<br />- DEBUG - this is the noisiest level.<br />- ERROR<br />- WARN<br />- NONE | NONE        |
  | sendMRAIDSupportParams  | If `true`, the SDK sends "`af=3,5`", indicating support for MRAID. | true        |

Custom key-value parameters
--------------------------------------

You can submit values for the extended (`c.xxx`) ad-call
parameters.

| **Parameter**           | **Method**          | **Description**                                              |
| ----------------------- | ------------------- | ------------------------------------------------------------ |
| custom<br />parameter   | setCustomParameter  | A custom user parameter auto-prepended with c..<br />You should provide the plain name of the parameter, such as xxx, which will be changed to c.xxx when sent. |
| custom <br />parameters | setCustomParameters | Custom user parameters, which consist of a dictionary of name-value parameter pairs, where each param name will be automatically prepended with c.. |
