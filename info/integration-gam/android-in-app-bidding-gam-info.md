# Google Ad Manager Integration

## Table of Contents

1. [SDKs Integration](#gam-sdk-integration)
2. [Order Setup](#order-setup)
3. [Mobile API](#mobile-api)
    - [Init SDK](#init-in-app-bidding-sdk)
    - [Banner](#banner-api)
    - [Interstitial](#interstitial-api)
    - [Rewarded](#rewarded-api)


## GAM SDK Integration

The prerequisite of the In-App Bidding integration with Google Ad Manager (GAM or DFP) is an installed GAM SDK. If you do not have the GAM SDK in the app yet, refer the [Google Documentation](https://developers.google.com/ad-manager/mobile-ads-sdk/android/quick-start) about the integration process. The In-App Bidding SDK was tested with GAM SDK 7.61.0. If you have any trouble with this or other versions, please contact the [OpenX Support](https://docs.openx.com/resources/support/).


## Order Setup

To integrate header bidding with GAM you have to prepare a specific Order following the [instructions](android-in-app-bidding-gam-order-setup.md) for particular ad type.

## Mobile API

<img src="../res/GAM-In-App-Bidding-Overview.png" alt="Pipeline Screenshot" align="center">

The OpenX In-App Bidding SDK provides an ability to integrate header bidding for the following ad types:

- Display Banner
- Display Interstitial
- Video Interstitial
- Rewarded Video
- Outstream Video

However, the OpenX In-App Bidding GAM facade provides only three types of API classes for these ads:

- **Banner API** - for **Display Banner** and **Outstream Video**
- **Interstitial API** - for **Display** and **Video** Interstitials
- **Rewarded API** - for **Rewarded Video**

For how to create an Apollo account and start using the Apollo SDK, see the [Getting Started](../android-in-app-bidding-getting-started.md) page first.

### Event Handlers

GAM Event Handlers is a set of classes that wrap the GAM Ad Units and manage them respectively to the In-App Bidding flow. These classes are provided in the form of library that could be added to the app via Gradle:

Root build.gradle
```
allprojects {
    repositories {
      ...
      maven { url "http://sdk.prod.gcp.openx.org/" }
      ...
    }
}
```
App module build.gradle:
```
implementation('com.openx:apollo-gam-event-handlers:x.x.x')
```

Or you can [download](https://storage.cloud.google.com/ox-cdn-prod-mobile/sdks/apollo/release/android/sdk/1.0.0/OpenX_Apollo_Android_GAM_Event_Handlers_1.0.0.zip) it manually and add it as any other regular library.


### Init In-App Bidding SDK

To start running bid requests you have to provide an **Account Id** for your organization on the Apollo server to the SDK:

```
OXSettings.setBidServerAccountId(YOUR_ACCOUNT_ID)
```

The best way to achieve this is by utilizing the `onCreate()` method of your Application class.

> **NOTE:** The account ID is an identifier of the **Stored Request** of your organization in the Apollo UI.


### Banner API

To integrate a banner ad, you need to implement three easy steps:


``` kotlin
// 1. Create banner custom event handler for GAM ad server.
val eventHandler = GamBannerEventHandler(requireContext(), GAM_AD_UNIT, GAM_AD_SIZE)

// 2. Create a bannerView instance and provide GAM event handler
bannerView = BannerView(requireContext(), configId, adSize, eventHandler)
// (Optional) set an event listener
bannerView?.setBannerListener(this)

// Add bannerView to your viewContainer
viewContainer?.addView(bannerView)

// 3. Execute ad loading
bannerView?.loadAd()
```

#### Step 1: Create Event Handler

GAM's event handlers are special containers that wrap GAM Ad Views and help to manage collaboration between GAM and OpenX views.

**Important:** You should create and use a unique event handler for each ad view.

To create the event handler you should provide a GAM Ad Unit Id and the list of available sizes for this ad unit.


#### Step 2: Create Ad View

**`BannerView`** - is a view that will display the particular ad. It should be added to the UI. To create it you should provide:

- **`configId`** - an ID of Stored Impression on the Apollo server
- **`eventHandler`** - the instance of the banner event handler

Also, you should add the instance of `BannerView` to the UI.

And assign the [listeners](../android-in-app-bidding-listeners.md) for processing ad events.


#### Step 3: Load the Ad

Simply call the `loadAd()` method to start the [In-App Bidding](../android-in-app-bidding-getting-started.md) flow. The In-App Bidding SDK starts the  bidding process right away.

#### Outstream Video

For **Outstream Video** you also need to specify video placement type of the expected ad:

``` kotlin
bannerView.videoPlacementType = PlacementType.IN_BANNER // or any other available type
```


### Interstitial API

To integrate interstitial ad you need to implement four easy steps:


``` kotlin
// 1. Create interstitial custom event handler for GAM ad server.
val eventHandler = GamInterstitialEventHandler(requireContext(), gamAdUnit)

// 2. Create interstitialAdUnit instance and provide GAM event handler
interstitialAdUnit = InterstitialAdUnit(requireContext(), configId, minSizePercentage, eventHandler)
// (Optional) set an event listener
interstitialAdUnit?.setInterstitialAdUnitListener(this)

// 3. Execute ad load
interstitialAdUnit?.loadAd()

//....

// 4. After ad is loaded you can execute `show` to trigger ad display
interstitialAdUnit?.show()

```

The way of displaying **Video Interstitial Ad** is almost the same with two differences:

- Need to customize the ad unit format
- No need to set up `minSizePercentage`

``` kotlin
// 1. Create interstitial custom event handler for GAM ad server.
val eventHandler = GamInterstitialEventHandler(requireContext(), gamAdUnit)

// 2. Create interstitialAdUnit instance and provide GAM event handler
interstitialAdUnit = InterstitialAdUnit(requireContext(), configId, AdUnitFormat.VIDEO, eventHandler)

// (Optional) set an event listener
interstitialAdUnit?.setInterstitialAdUnitListener(this)

// 3. Execute ad load
interstitialAdUnit?.loadAd()

//....

// 4. After ad is loaded you can execute `show` to trigger ad display
interstitialAdUnit?.show()

```


#### Step 1: Create Event Handler

GAM's event handlers are special containers that wrap the GAM Ad Views and help to manage collaboration between GAM and OpenX views.

**Important:** you should create and use a unique event handler for each ad view.

To create an event handler you should provide a GAM Ad Unit.

#### Step 2: Create Interstitial Ad Unit

**`InterstitialAdUnit`** - is an object that will load and display the particular ad. To create it you should provide:

- **`configId`** - an ID of Stored Impression on the Apollo server
- **`minSizePercentage`** - specifies the minimum width and height percent an ad may occupy of a device’s real estate.
- **`eventHandler`** - the instance of the interstitial event handler

Also, you can assign the [lisnteners](../android-in-app-bidding-listeners.md) for processing ad events.

> **NOTE:** `minSizePercentage` plays an important role in a bidding process for display ads. If the provided space is not enough demand partners will not respond with the bids.


#### Step 3: Load the Ad

Simply call the `loadAd()` method to start the [In-App Bidding](../android-in-app-bidding-getting-started.md) flow. The ad unit will load an ad and will wait for explicit instructions to display the Interstitial Ad.


#### Step 4: Show the Ad when Ready


The most convenient way to determine if the interstitial ad is ready for displaying is to listen to the particular [listener](../android-in-app-bidding-listeners.md) method:

``` kotlin
override fun onAdLoaded(interstitialAdUnit: InterstitialAdUnit) {
//Ad is ready for display
}
```

### Rewarded API

To display a Rewarded Ad you need to implement four easy steps:


``` kotlin
// 1. Create rewarded custom event handler for GAM ad server.
val eventHandler = GamRewardedEventHandler(requireActivity(), gamAdUnitId)

// 2. Create rewardedAdUnit instance and provide GAM event handler
rewardedAdUnit = RewardedAdUnit(requireContext(), configId, eventHandler)

// (Optional) set an event listener
rewardedAdUnit?.setRewardedAdUnitListener(this)

// 3. Execute ad load
rewardedAdUnit?.loadAd()

//...

// 4. After ad is loaded you can execute `show` to trigger ad display
rewardedAdUnit?.show()
```

The way of displaying the **Rewarded Ad** is exactly the same as for the Interstitial Ad. You can customize an ad type:


To be notified when a user earns a reward - implement the `RewardedAdUnitListener` interface:

``` kotlin
 fun onUserEarnedReward(rewardedAdUnit: RewardedAdUnit)
```

The actual reward object is stored in the `RewardedAdUnit`:

``` kotlin
val reward = rewardedAdUnit.getUserReward()
```

#### Step 1: Create Event Handler

GAM's event handlers are special containers that wrap the GAM Ad Views and help to manage collaboration between GAM and OpenX views.

**Important:** You should create and use a unique event handler for each ad view.

To create an event handler you should provide a GAM Ad Unit.


#### Step 2: Create Rewarded Ad Unit

**`RewardedAdUnit`** - is an object that will load and display the particular ad. To create it you should provide:

- **`configId`** - an ID of Stored Impression on the Apollo server
- **`eventHandler`** - the instance of the rewarded event handler

Also, you can assign the [listener](../android-in-app-bidding-listeners.md) for processing ad events.


#### Step 3: Load the Ad

Simply call the `loadAd()` method to start the [In-App Bidding](../android-in-app-bidding-getting-started.md) flow. The ad unit will load an ad and will wait for explicit instructions to display the Rewarded Ad.


#### Step 4: Show the Ad when Ready


The most convenient way to determine if the ad is ready for displaying is to listen for a particular [listener](../android-in-app-bidding-listeners.md) method:

``` kotlin
override fun onAdLoaded(rewardedAdUnit: RewardedAdUnit) {
//Ad is ready for display
}
```
