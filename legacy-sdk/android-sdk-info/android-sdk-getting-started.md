Getting started
===============

To get started with the OpenX Mobile Android SDK, make sure you meet the following requirements and review the [integration steps](#integration-process-overview) outlined below. For new features and updates, see [Release notes.](rn-android-sdk-main.md)

To integrate the OpenX Mobile Android SDK 4.8.x, make sure you use the following:

- For display ads, the ad unit ID and OpenX delivery domain provided by OpenX.

- For video interstitial and opt-in video ads, the domain and ad unit id provided by OpenX. - For video interstitial and opt-in video ads, the domain an ad unit id, provided by OpenX. For more information, contact your OpenX account representative.

- Android SDK version 4.x.x (API level 14) to 9.x.x (API level 28). Beta versions are not supported.

- OpenX Mobile Android SDK version 4.7.1 4.8.1 or later (see [Release notes](rn-android-sdk-main.md)). These versions provide General Data Protection Regulation (GDPR) compliance.

  > **Important:** For GDPR compliance, every publisher must also integrate with a Consent Management Provider (CMP) that is approved by the Interactive Advertising Bureau (IAB). See the [IAB's CMP vendor list](http://advertisingconsent.eu/).

Ad Formats and Tips
-------------------------

**Ad Formats and  Integration:**

- [Integrating the SDK with your app](android-sdk-integration.md)
- [Banner ad integration](android-sdk-banner-integration.md)
- [Interstitial ad integration](android-sdk-interstitial-integration.md)
- [Video interstitial integration](android-sdk-video-interstitial-integration.md)
- [Opt-in video integration](android-sdk-video-optin-integration.md)
- [Video 300x250 integration](android-sdk-video-ad-view-integration.md) (New in 4.10)

**Behavior and Parameterization**

- [Callbacks](android-sdk-controller-callbacks.md)
- [Parameters](android-sdk-request-params.md)

**Ad Network Adapters:**

- [MoPub Adapter](android-sdk-mopub-adapter.md)
- [AdMob Adapter](android-sdk-admob-adapter.md)

**Integration Features and Tips:**

- [Launching the demo app](android-sdk-demo-app-launch.md)
- [Testing and troubleshooting](android-sdk-self-test.md)
- [Ad Quality](android-sdk-ad-quality.md)



Integration process overview
-----------------------------------------

To integrate the OpenX Mobile Android SDK, complete the following steps. To explore the provided sample implementations, [download](https://sdk.prod.gcp.openx.org/android/4.13.0/OpenX_Mobile_SDK_Android_4.13.0.zip) the SDK and [launch the demo app](android-sdk-demo-app-launch.md).

1.  Make sure you meet the above [requirements](#requirements).
1.  [Integrate the SDK with your app](android-sdk-integration.md) by updating the Android manifest.
1.  If you want to use OpenX as a source of demand on MoPub and/or AdMob via their respective SDKs, build the [MoPub](android-sdk-mopub-adapter.md) and/or [AdMob](android-sdk-admob-adapter.md) SDK adapter and skip to step 5.
1.  Integrate the desired ad unit into your app:
    -   [Banner ad integration](android-sdk-banner-integration.md)
    -   [Interstitial ad integration](android-sdk-interstitial-integration.md)
    -   [Video interstitial integration](android-sdk-video-interstitial-integration.md)
    -   [Opt-in video integration](android-sdk-video-optin-integration.md)
    -   [300x250 video](android-sdk-info/android-sdk-video-ad-view-integration.md) (New in 4.10)
7.  [Test](android-sdk-self-test.md) your integration and notify OpenX to enable live traffic.


SDK zip file contents
-----------------------------

The OpenX Mobile Android SDK zip file contains the items listed below.

| **Item**                  | **Description**                                              |
| ------------------------- | ------------------------------------------------------------ |
| OpenXApolloSDK-x.x.x.aar      | The SDK .aar file.                                           |
| omsdk-android-x.x.x.aar   | The [OpenMeasurement SDK](https://iabtechlab.com/standards/open-measurement-sdk/) .aar file.                           |
| OpenX_Mobile_Demo_Android | A [demo app](android-sdk-demo-app-launch.md) that uses OpenX's SDK library and provides several integration scenarios. |
| Mediation Adapters        | The [MoPub SDK adapters](android-sdk-mopub-adapter.md) and [AdMob SDK adapters](android-sdk-admob-adapter.md). |
| docs                      | End user license agreement, readme file that directs you to this page, and integration guide for the downloaded OpenX Mobile Android SDK version. |

> **Note:** The zip file does not contain Javadoc because the class reference is included in the demo app and is available through your integrated development environment (IDE).
