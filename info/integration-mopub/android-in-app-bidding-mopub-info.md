# MoPub Integration

The integration of Apollo SDK with MoPub assumes that publisher has an account on MoPub and has already integrated the MoPub SDK into the app project. 

If you do not have MoPub SDK in the app yet, refer the the [MoPub's Documentation](https://github.com/mopub/mopub-android-sdk).

If you have any troubles with integration contact [Apollo Support](https://www.openx.com/prebid/#form).

## Order Setup 

To integrate header bidding into MoPub you have to prepare a specific Order following the [instructions](android-in-app-bidding-mopub-order-setup.md) for particular ad kind.

### Rendering of vanilla prebid orders

If you want to run In-App Biding with Apollo using your Prebid orders on MoPub you do not have to change anything on MoPub. **Apollo SDK is able to work with prebid orders**. Just replace Prebid SDK with Apollo SDK and follow the current integration instructions. 

> Subsequently, we recommend to switch to Apollo orders in order to get better rendering, measurement and targeting.

## MoPub Integration Overview

The integration of header bidding into MoPub monetization is based on MoPub's Mediation feature. 

<img src="../res/Apollo-In-App-Bidding-Overview-MoPub.png" alt="Pipeline Screenshot" align="center">

**Steps 1-2** Apollo SDK makes a bid request. Apollo server runs an auction and returns the winning bid to the SDK.

**Step 3** Apollo SDK via MoPub Adapters Framework sets up targeting keywords into the MoPub's ad unit.

**Step 4** MoPub SDK makes an ad request. MoPub returns the winner of the waterfall.

**Step 5** If Apollo's creative won then the MoPub SDK will instantiate respective Apollo Adapter which will render the winning bid.

**Step 6** The winner is displayed in the App with the respective rendering engine.

Apollo SDK provides ability to integrate header bidding for these ad kinds:

- Display Banner
- Display Interstitial
- Native
- [Native Styles](../integration-mopub/ios-in-app-bidding-mopub-native-integration.md)
- Video Interstitial 
- Rewarded Video

They can be integrated using these API categories.

- [**Banner API**](#Banner-API) - for **Display Banner** 
- [**Interstitial API**](#Interstitial-API) - for **Display** and **Video** Interstitials
- [**Rewarded API**](#Rewarded-API) - for **Rewarded Video**
- [**Native API**](android-in-app-bidding-mopub-native-integration.md)


## Init Apollo SDK

Provide an **Account Id** of your organization on Apollo server:

```
ApolloSettings.setAccountId(YOUR_ACCOUNT_ID)
```

The best place to do it is the `onCreate()` method of your Application class.

The account ID is an identifier of the **Stored Request** of your organization on the Apollo UI. 

### Apollo Adapters

Adapters for Apollo SDK are open source classes that serve like proxies between MoPub SDK and any other one. For more details about Mediation and Adapters read the [MoPub's Documentation](https://developers.mopub.com/networks/integrate/mopub-network-mediation-guidelines/).

To integrate adapters for In-App Bidding SDK just add the following lines in your build.gradle files:
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
implementation('com.openx:apollo-mopub-adapters:x.x.x')
```

Or you can [download](https://storage.cloud.google.com/ox-cdn-prod-mobile/sdks/apollo/release/android/event-handlers/MoPub/1.2.0/OpenX_Apollo_Android_MoPub_Adapters_1.2.0.zip) it manually and add as any other regular library.

## Banner API

To display an ad you need to implement these easy steps:


``` kotlin
private fun initBanner() {
    // 1. Create and initialize MoPubView instance
    bannerView = MoPubView(requireContext())
    bannerView?.setAdUnitId(moPubAdUnit)
    bannerView?.bannerAdListener = this

    // Add moPubView to your viewContainer
    viewContainer?.addView(bannerView)

    val builder = SdkConfiguration.Builder(moPubAdUnit)
    MoPub.initializeSdk(requireContext(), builder.build()) {
        fetchAdUnit(configId, AdSize(320, 50))
    }
}

private fun fetchAdUnit(configId: String, size: AdSize) {
    if (bannerAdUnit == null) {
        // 2. initialize MoPubBannerAdUnit
        bannerAdUnit = MoPubBannerAdUnit(requireContext(), configId, size)
    }
    // 3. Run an Header Bidding auction on Apollo and provide MoPubView as parameter. It is important to execute this method after MoPub SDK initialization.
    bannerAdUnit?.fetchDemand(bannerView!!) {
        // 4. execute MoPubView `loadAd` when receiving a valid demand result
        bannerView?.loadAd()
    }
}
```

#### Step 1: Create Ad View

In the scenario with MoPub integration the MoPub's SDK plays the central role in managing ad views in the application's UI. You have to create and place MoPub's Ad View into the app page. If a winning bid on Apollo wins in the MoPub waterfall it will be rendered via Mediation in the place of original MoPub's Ad View by Apollo SDK.

#### Step 2: Create Ad Unit

Create the **MoPubBannerAdUnit** object with parameters:

- **configId** - an ID of Stored Impression on the Apollo server
- **size** - the size of the ad unit which will be used in the bid request.

#### Step 3: Fetch Demand

To run an auction on Apollo run the `fetchDemand()` method which performs several actions:

- Makes a bid request to Apollo
- Sets up the targeting keywords to the MoPub's ad unit
- Passes the winning bid to the MoPub's ad unit
- Returns the result of bid request for future processing

#### Step 4: Load the Ad

When the bid request has completed, the responsibility of making the Ad Request is passed to the publisher. That is why you have to invoke `loadAd()` on the MoPub's Ad View explicitly in the completion handler of `fetchDemand()`.

#### Step 5: Rendering

If the Apollo bid wins on MoPub it will be rendered by `OpenXApolloBannerAdapter`. You don't have to do anything for this.  Just make sure that your order had been set up correctly and an adapter is added.

## Interstitial API

To display an ad you need to implement these easy steps:

``` kotlin
private fun initInterstitial() {
    // 1. Create and initialize MoPubInterstitial instance
    moPubInterstitial = MoPubInterstitial(requireActivity(), adUnit)
    moPubInterstitial?.interstitialAdListener = this

    // 2. Initialize MoPubInterstitialAdUnit
    moPubInterstitialAdUnit = MoPubInterstitialAdUnit(requireContext(), configId, minSizePercentage)

    val builder = SdkConfiguration.Builder(adUnit)
    MoPub.initializeSdk(requireContext(), builder.build()) {
        fetchInterstitial()
    }
}

private fun fetchInterstitial() {
    // 3. Execute `fetchDemand` method and provide MoPubInterstitial as parameter. It is important to execute this method after MoPub SDK initialization.
    moPubInterstitialAdUnit?.fetchDemand(moPubInterstitial!!) {
        // 4. Execute MoPubInterstitial `load` when receiving a valid demand result
        moPubInterstitial?.load()
    }
}
    
    
//...
// After ad is loaded you can execute `show` to trigger ad display
moPubInterstitial?.show()
```

The way of displaying **Video Interstitial Ad** is almost the same with two differences:

- Need customize the ad unit kind
- No need to set up `minSizePercentage`

``` kotlin
private fun initInterstitial() {
    // 1. Create and initialize MoPubInterstitial instance
    moPubInterstitial = MoPubInterstitial(requireActivity(), adUnit)
    moPubInterstitial?.interstitialAdListener = this

    // 2. Initialize MoPubInterstitialAdUnit and provide VIDEO AdUnitFormat
    moPubInterstitialAdUnit = MoPubInterstitialAdUnit(requireContext(), configId, AdUnitFormat.VIDEO)

    val builder = SdkConfiguration.Builder(adUnit)
    MoPub.initializeSdk(requireContext(), builder.build()) {
        fetchInterstitial()
    }
}

private fun fetchInterstitial() {
    // 3. Execute `fetchDemand` method and provide MoPubInterstitial as parameter. It is important to execute this method after MoPub SDK initialization.
    moPubInterstitialAdUnit?.fetchDemand(moPubInterstitial!!) {
        // 4. Execute MoPubInterstitial `load` when receiving a valid demand result
        moPubInterstitial?.load()
    }
}

//...
// After ad is loaded you can execute `show` to trigger ad display
moPubInterstitial?.show()
```

#### Step 1: Create Ad View

In the scenario with MoPub integration the MoPub SDK plays the central role in managing ad views in the application's UI. If a winning bid on Apollo wins in the MoPub waterfall it will be rendered via Mediation by Apollo SDK.

#### Step 2: Create Ad Unit

Create the **MoPubInterstitialAdUnit** object with parameters:

- **configId** - an ID of Stored Impression on the Apollo server

#### Step 3: Fetch Demand

To run an auction on Apollo run the`fetchDemand()` method which performs several actions:

- Makes a bid request to Apollo
- Sets up the targeting keywords to the MoPub's ad unit
- Passes the winning bid to the MoPub's ad unit
- Returns the result of bid request for future processing

#### Step 4: Load the Ad

When the bid request has been completed the responsibility of making the Ad Request is passed on the publisher. That is why you have to invoke the `loadAd()` on the MoPub Ad View explicitly in the completion handler of the `fetchDemand()`.


#### Step 5: Rendering

If the Apollo bid wins on MoPub it will be rendered by `OpenXApolloInterstitialAdapter`. You do not have to do anything for this.  Just make sure that your order had been set up correctly and an adapter is added.


However, due to the expiration, the ad could become invalid with time. So it is always useful to check it with `interstitial?.isReady` before display.


## Rewarded API

To display an ad you need to implement these easy steps:

``` kotlin
private fun initRewarded() {
    // 1. Create MoPubRewardedVideoAdUnit instance
    rewardedAdUnit = MoPubRewardedVideoAdUnit(requireContext(), adUnitId, configId)
    
    // 2. Initialize MoPub SDK and MoPubRewardedVideoManager.
    val builder = SdkConfiguration.Builder(adUnitId)
    MoPubRewardedVideoManager.init(requireActivity())
    MoPubRewardedVideoManager.updateActivity(requireActivity())
    MoPubRewardedVideos.setRewardedVideoListener(this)
    MoPub.initializeSdk(requireContext(), builder.build()) {
        fetchRewarded(adUnitId)
    }
}

private fun fetchRewarded(adUnitId: String) {
    // 3. Execute `fetchDemand` method and keywords Map as parameter. It is important to execute this method after MoPub SDK initialization.
    rewardedAdUnit?.fetchDemand(keywordsMap) {
        val keywordsString = convertMapToMoPubKeywords(keywordsMap)
        val params = MoPubRewardedVideoManager.RequestParameters(keywordsString)

        // 4. After creating RequestParameters from keywordsMap you can execute rewardedVideo loading
        MoPubRewardedVideos.loadRewardedVideo(adUnitId, params, null)
    }
}

//...
// After ad is loaded you can execute `show` to trigger ad display
MoPubRewardedVideos.showRewardedVideo(adUnitId)
```

#### Step 1: Create an Rewarded Ad Unit

Create the **MoPubRewardedVideoAdUnit** object with parameters:

- **configId** - an ID of Stored Impression on the Apollo server

#### Step 2: Fetch Demand

To run an auction on Apollo run the `fetchDemand()` method which does several things:

- Makes a bid request to Apollo
- Sets up the targeting keywords
- Returns the result of bid request for future processing

#### Step 3: Load the Ad

When the bid request has completed, the responsibility of making the Ad Request is passed to the publisher. That is why you have to invoke the `loadAd()` of the MoPub's Ad View explicitly in the completion handler of the `fetchDemand()`.


#### Step 5: Rendering

If the Apollo bid wins on MoPub it will be rendered by `OpenXApolloRewardedVideoAdapter`. You do not have to do anything for this.  Just make sure that your order had been set up correctly and an adapter is added.








