# MoPub: Native Ads Integration

## Unified Native Ads

The rendering using native components is not supported yet. This ad format is scheduled for the next version - **Apollo SDK 1.2**.

## Native Styles 
[See MoPub Integration page](../integration-mopub/android-in-app-bidding-mopub-info.md) for more info about MoPub order setup and Adapter integration.

To display an ad you need to implement these easy steps:

``` kotlin
private fun initBanner() {
    // 1. Create and initialize MoPubView instance
    bannerView = MoPubView(requireContext())
    bannerView?.setAdUnitId(moPubAdUnit)
    bannerView?.bannerAdListener = this
    
    // 2. initialize MoPubBannerAdUnit
    if (bannerAdUnit == null) {
        bannerAdUnit = MoPubBannerAdUnit(requireContext(), configId, AdSize(width, height))        
    }
    
    // 3. Provide NativeAdConfiguration
    val nativeAdConfiguration = createNativeAdConfiguration()
    bannerAdUnit?.setNativeAdConfiguration(nativeAdConfiguration)

    // Add moPubView to your viewContainer
    viewContainer?.addView(bannerView)

    val builder = SdkConfiguration.Builder(moPubAdUnit)
    MoPub.initializeSdk(requireContext(), builder.build()) {
        
        // 4. Run an Header Bidding auction on Apollo and provide MoPubView as parameter. It is important to execute this method after MoPub SDK initialization.
        bannerAdUnit?.fetchDemand(bannerView!!) {
            // 5. execute MoPubView `loadAd` when receiving a valid demand result
            bannerView?.loadAd()
        }
    }
}
```

#### Step 1: Create Ad View

You have to create and place MoPub's Ad View into the app page.


#### Step 2: Create Ad Unit

Create the **MoPubBannerAdUnit** object with parameters:

- **configId** - an ID of Stored Impression on the Apollo server
- **size** - the size of the ad unit which will be used in the bid request.


#### Step 3: Create and provide NativeAdConfiguration

NativeAdConfiguration creation example:
``` kotlin
private fun createNativeAdConfiguration(): NativeAdConfiguration {
    val nativeAdConfiguration = NativeAdConfiguration()
    nativeAdConfiguration.contextType = NativeAdConfiguration.ContextType.SOCIAL_CENTRIC
    nativeAdConfiguration.placementType = NativeAdConfiguration.PlacementType.CONTENT_FEED
    nativeAdConfiguration.contextSubType = NativeAdConfiguration.ContextSubType.GENERAL_SOCIAL

    val methods = ArrayList<NativeEventTracker.EventTrackingMethod>()
    methods.add(NativeEventTracker.EventTrackingMethod.IMAGE)
    methods.add(NativeEventTracker.EventTrackingMethod.JS)
    val eventTracker = NativeEventTracker(NativeEventTracker.EventType.IMPRESSION, methods)
    nativeAdConfiguration.addTracker(eventTracker)

    val assetTitle = NativeAssetTitle()
    assetTitle.len = 90
    assetTitle.isRequired = true
    nativeAdConfiguration.addAsset(assetTitle)

    val assetIcon = NativeAssetImage()
    assetIcon.type = NativeAssetImage.ImageType.ICON
    assetIcon.wMin = 20
    assetIcon.hMin = 20
    assetIcon.isRequired = true
    nativeAdConfiguration.addAsset(assetIcon)

    val assetImage = NativeAssetImage()
    assetImage.hMin = 20
    assetImage.wMin = 200
    assetImage.isRequired = true
    nativeAdConfiguration.addAsset(assetImage)

    val assetData = NativeAssetData()
    assetData.len = 90
    assetData.type = NativeAssetData.DataType.SPONSORED
    assetData.isRequired = true
    nativeAdConfiguration.addAsset(assetData)

    val assetBody = NativeAssetData()
    assetBody.isRequired = true
    assetBody.type = NativeAssetData.DataType.DESC
    nativeAdConfiguration.addAsset(assetBody)

    val assetCta = NativeAssetData()
    assetCta.isRequired = true
    assetCta.type = NativeAssetData.DataType.CTA_TEXT
    nativeAdConfiguration.addAsset(assetCta)

    return nativeAdConfiguration
}
```
See more NativeAdConfiguration options [here](../native/android-native-ad-configuration.md).

#### Step 4: Fetch Demand

To run an auction on Apollo run the `fetchDemand()` method which performs several actions:

- Makes a bid request to Apollo
- Sets up the targeting keywords to the MoPub's ad unit
- Passes the winning bid to the MoPub's ad unit
- Returns the result of bid request for future processing

#### Step 5: Load the Ad

When the bid request has completed, the responsibility of making the Ad Request is passed to the publisher. That is why you have to invoke `loadAd()` on the MoPub's Ad View explicitly in the completion handler of `fetchDemand()`.

