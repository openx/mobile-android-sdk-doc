# Getting started with In-App Bidding


The In-App Bidding SDK provides an access to the header bidding power hosted by OpenX. It allows publishers to increase the revenue by running a parallel auction on the server-side, inject a winning bid into the predefined waterfall, and render premium ad formats with the best quality according to the latest industry standards.

## In-App Bidding Overview  


<img src="res/In-App-Bidding-Overview.png" alt="Pipeline Screenshot" align="center">

1. In-App Bidding SDK sends the bid request to the OpenX Apollo server.
2. Apollo server runs the header bidding auction among preconfigured demand partners.
3. In-App Bidding SDK sets up the targeting keywords of the winning bid to the ad unit of Primary Ad Server SDK.
4. Primary Ad Server SDK sends the ad request to the Primary Ad Server.
5. Depending on the ad response the In-App Bidding SDK renders the winning bid or allows the Primary Ad Server SDK to show its own winning ad in the internal waterfall.  

> Note: Steps 3 and 4 are relevant when integrating with the Primary Ad Server. In case of the Pure In-App Bidding scenario, the winning bid will be rendered immediately after it was received.


## Supported Ad Formats

The OpenX In-App Bidding SDK supports next premium ad formats:

 - Display Banner
 - Display Interstitial
 - Video Interstitial
 - Rewarded Video
 - Outstream Video (for GAM and Pure In-App Bidding)

## Apollo Setup

To start running a header bidding auction you need to register on the Apollo server and create configurations for ad units and demand partners. Visit the [Apollo site](https://www.openx.com/prebid/) for details.

Before the integration of the In-App Bidding SDK you should have:

- **Apollo Account ID** - an identifier of the **Stored Request** of your organization on the Apollo UI.
- **Configuration ID** - an identifier of the **Stored Impression** which contains information about bidders for a particular ad unit in the app. You need as many IDs as many different ad units you want to integrate.


## Init Apollo SDK

To use the In-App Bidding SDK, you first need to [integrate](android-sdk-integration.md) the Apollo SDK.

To start running bid requests, you have to provide the SDK an Account Id for your organization on the Apollo server.

```
OXSettings.setBidServerAccountId(YOUR_ACCOUNT_ID)
```

The best way to achieve this is by utilizing the `onCreate()` method of your Application class. The account ID is an identifier of the **Stored Request** of your organization on the Apollo UI.


## Integration Scenarios and Tips


Depending on the Primary Ad Server used, the In-App Bidding SDK supports the following types of integrations:

- With [Google Ad Manager (GAM)](integration-gam/android-in-app-bidding-gam-info.md) as a Primary Ad Server
- With [MoPub](integration-mopub/android-in-app-bidding-mopub-info.md) as a Primary Ad Server
- [Pure In-App Bidding](integration-pb/android-in-app-bidding-pb-info.md) integration without Primary Ad Server


## Integration Tips


**Integration Features and Tips:**

- To inspect the code integration, download and run a [Demo Application](https://storage.cloud.google.com/ox-cdn-prod-mobile/sdks/apollo/release/android/sdk/1.0.0/OpenX_Apollo_SDK_Android_Demo_1.0.0.zip)
- [Testing and troubleshooting](android-sdk-self-test.md)
