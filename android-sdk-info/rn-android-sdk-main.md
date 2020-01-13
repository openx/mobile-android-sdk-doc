OpenX Mobile Android SDK Release Notes
======================================


Version 4.11
-------------

Jan 14, 2020

**Features**

* Support the deeplink+. The SDK is able to manage the app-targeting campaigns.
* Track viewable impressions. The SDK tracks impressions according to the MRC Guidelines.
* Support CCPA.
* Update the Transparency and Consent Framework (GDPR) to TCF v2.
* Support Android Q.
* Support Vertical Video ads.
* Support MoPub’s custom AdapterConfiguration.

**Changes and Improvements**

* The configuration settings for ad requests was changed. The access to the sensitive fields was limited to prevent inappropriate bidding.
* The behavior between several active ad views was improved. Now you can place different ad units on the same view.
* Upgrade the minimum supported Android version API 16 (Android 4.1),
* Improvements and corrections for the logging system.

Version 4.10
-------------

July 30, 2019


**Features**

* SSL connection for all ad requests. Starting from 4.10 OpenX SDK performs only secure ad requests.
* 300x250 Video ad format. Now publishers may integrate the video ads into their UI.

**Changes and Improvements**

* Different initialization of video ad classes: Provide **domain** and **ad unit ID** values instead of **vastURL**.
* Improved UI/UX for interstitial ads.
* Updates and improved support for the OMSDK library.
* Improvements and fixes for MRAID behavior.
* Corrections for pre-caching of video ads.
* Availability of Ad Mob adapter.

Android
- Support for Application context for basic ad functionality.
- Migration to AndroidX.
- Replacement of VideoView with Exo Player for video ads.
- Removal of deprecated AdView.

Version 4.9.1
-------------

June 23, 2019

This version of the OpenX Mobile Android SDK includes a critical bug fix to resolve crashes for display ads with an unlimited number of refreshes.

Version 4.9
-------------

June 6, 2019

The OpenX Mobile SDK features enhancements that improve bidding insights and user experience:

-   Integration with the [IAB’s Open Measurement SDK](https://iabtechlab.com/standards/open-measurement-sdk/) provides viewability tracking to enable marketers to reach users in high-viewability inventory across all their devices.
-   An AdChoices icon appears with every mobile ad served through the SDK. This icon indicates that an ad is being displayed. If the user clicks the icon, they are redirected to the [OpenX privacy page](https://www.openx.com/legal/privacy-policy/), which shows the information that is being collected about them and that may be used for targeting them in future ads.
-   Video precaching enables ads to load before viewing so that they can run without delay.

**Major design changes and removed features**

-   AdView is deprecated and will be removed in version 4.10.
-   The functionality of AdView is now available in two new features: BannerView and InterstitialView.
-   The AndroidSDKDemo app has been updated.
-   READ_PHONE_STATE permission has been removed.
-   Use of ANDROID_ID has been removed.

**Major bug fixes**

-   Identical impressions are no longer sent when autorefresh is set to 0.
-   MRAID is now supported for end cards.
-   Completed videos with no end cards automatically close.

Version 4.8.1
-------------

October 23, 2018

This version of the OpenX Mobile Android SDK includes a required fix for
compliance with Google Play Store policies. You must update your OpenX
Mobile Android SDK to this version for your app to be compliant.

Version 4.8.0
---------------------

August 24, 2018

This version of the OpenX Mobile Android SDK includes:

-   **Opt-in** **(rewarded)** **video with end card support.** This feature in the SDK is in Beta release only. For details, see [Opt-in video integration](android-sdk-video-optin-integration.md).
-   **AdMob adapter integration.** For details, see [AdMob adapter](android-sdk-admob-adapter.md).

Version 4.7.1
-------------

October 15, 2018

This version of the OpenX Mobile Android SDK includes a required fix for compliance with Google Play Store policies. You must update your OpenX Mobile Android SDK to this version for your app to be compliant.

Version 4.7.0
-------------

May 11, 2018

This version of the OpenX Mobile Android SDK includes:

-   **IAB GDPR Transparency and Consent Framework compliance.** The OpenX Mobile Android SDK now retrieves the user consent values set by an IAB-compliant Consent Management Provider (CMP) SDK. See the [IAB website](https://www.iab.com/news/iab-europe-releases-gdpr-transparency-consent-framework-public-comment/) for more details about the proposed framework.

Version 4.6.2
-------------

April 4, 2018

This version of the OpenX Mobile Android SDK includes bug fixes.

Version 4.6.1
-------------

January 16, 2018

This version of the OpenX Mobile Android SDK includes bug fixes.

Version 4.6.0
-------------

October 4, 2017

This version of the OpenX Mobile Android SDK includes:

-   Support for [video interstitial ads](android-sdk-video-interstitial-integration.md).
-   Improved [exception messages](android-sdk-self-test.md#ad-exception-types-and-examples) for troubleshooting.
-   Bug fixes.

Version 4.5.0
-------------

August 23, 2017

The OpenX Mobile Android SDK has been completely re-architected to
provide a faster, safer user experience. It includes:

-   Support for [flex ads](android-sdk-flex-ads.md).
-   Ability to mediate OpenX in MoPub using a [MoPub SDK adapter](android-sdk-mopub-adapter.md).
-   A unique `transactionId` for identifying an ad, so that you can troubleshoot ad quality issues.
-   Improved MRAID support.

The OpenX MoPub adapter has been tested on MoPub SDK version 4.14.0.
