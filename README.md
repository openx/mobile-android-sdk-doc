# OpenX Apollo Android SDK

Apollo SDK is a part of OpenX Apollo platform which implements an In-App Bidding approach and gives opportunities to buyers to compete on your inventory at parallel bidding auctions. 

The current SDK version is **1.2.0**.
Go to [release notes](info/android-in-app-bidding-release-notes.md) for all SDK versions.

> **_NOTE:_**  The documentation for the legacy OpenX SDK is available [here](legacy-sdk/README.md).

## Quick Start


#### Gradle Integration

To add the Apollo SDK dependency, open your project and update the app module’s build.gradle to have the following repositories and dependencies:

```
allprojects {
    repositories {
      ...
      maven { url "http://sdk.prod.gcp.openx.org/" }
      ...
    }
}

// ...

dependencies {
    ...
    implementation('com.openx:apollo-sdk:x.x.x')
    ...
}
```

#### Download SDK and demo applications

- [OpenX In-App Bidding SDK](https://storage.cloud.google.com/ox-cdn-prod-mobile/sdks/apollo/release/android/sdk/1.2.0/OpenX_Apollo_SDK_Android_1.2.0.zip) and [Demo Application](https://storage.cloud.google.com/ox-cdn-prod-mobile/sdks/apollo/release/android/sdk/1.2.0/OpenX_Apollo_SDK_Android_Demo_1.2.0.zip)
- [MoPub Adapters](https://storage.cloud.google.com/ox-cdn-prod-mobile/sdks/apollo/release/android/event-handlers/MoPub/1.2.0/OpenX_Apollo_Android_MoPub_Adapters_1.2.0.zip) and [GAM Event Handlers](https://storage.cloud.google.com/ox-cdn-prod-mobile/sdks/apollo/release/android/event-handlers/GAM/1.2.0/OpenX_Apollo_Android_GAM_Event_Handlers_1.2.0.zip)


## Apollo SDK Overview

For requirements and integration overview, see [Getting started with In-App Bidding](info/android-in-app-bidding-getting-started.md).

Here are key capabilities of the Android In-App Bidding SDK:

-   **Integration Scenarios**
    - [Google Ad Manager](info/integration-gam/android-in-app-bidding-gam-info.md)
    - [MoPub](info/integration-mopub/android-in-app-bidding-mopub-info.md)
    - [Pure In-App Bidding](info/integration-apollo/android-in-app-bidding-pb-info.md)


<img src="info/res/IAB_Cert.png" alt="Pipeline Screenshot" height="320" width="320" align="right">

-   **Support of these premium ad formats:**
    -   Banner
    -   Interstitial
    -   [**NEW**] [Native](info/android-in-app-bidding-native-guidelines-info.md) 
    -   Rich media and MRAID 3.0 support
    -   Video Interstitial
    -   Rewarded Video
    -   Outstream Video
-  **Open Measurement Support.** The In-App Bidding SDK is based on the former OpenX SDK which is certificated with IAB, IAS and MOAT.
-   **Direct SDK integration**. Allows you to pass first-party app data,
    user data, device data, and location data.  
-   **Privacy Regulation Compliance**. The In-App Bidding SDK meets **GDPR**, **CCPA**, **COPPA** requirements according to the IAB specifications.
-   **App targeting campaigns**. With the [support of deeplink+](info/android-sdk-deeplinkplus.md) SDK is able to manage the ads with premium UX for retargeting campaigns.
-    **Targeting**. Use [custom ad parameters](info/android-sdk-parameters.md) to increase the chance to win an impression and to improve ad views' UX.
-   **Tracking of render impression**. The OpenX In-App Bidding SDK tracks [render impressions](info/android-sdk-impression-tracking.md) according to the IAB Measurement Guidelines for all managed ads. Ads rendered by Primary Ad Server SDK track an impression beacon according to the internal algorithms.
-   **Demo app** that serves as an implementation guide and test environment.
-   **Fast and seamless integration.**


## Contact Us

If you have any questions or need help, go to [Apollo Support](https://www.openx.com/prebid/#form).
