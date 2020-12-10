# GAM: Native Ads Integration

## Unified Native Ads

The rendering using native components is not supported yet. This ad format is scheduled for the next version - **Apollo SDK 1.2**.

## Native Styles 

[See Google Ad Manager Integration page](android-in-app-bidding-gam-info.md) for more info about GAM order setup and GAM Event Handler integration.

To integrate a banner ad you need to implement three easy steps:

``` kotlin
// 1. Create banner custom event handler for GAM ad server.
val eventHandler = GamBannerEventHandler(requireContext(), GAM_AD_UNIT, GAM_AD_SIZE)

// 2. Create a bannerView instance and provide GAM event handler
bannerView = BannerView(requireContext(), configId, eventHandler)
// (Optional) set an event listener
bannerView?.setBannerListener(this)

// 3. Provide NativeAdConfiguration
val nativeAdConfiguration = createNativeAdConfiguration()
bannerView?.setNativeAdConfiguration(nativeAdConfiguration)

// Add bannerView to your viewContainer
viewContainer?.addView(bannerView)

// 4. Execute ad loading
bannerView?.loadAd()
```

#### Step 1: Create Event Handler

GAM's event handlers are special containers that wrap GAM Ad Views and help to manage collaboration between GAM and OpenX views.

**Important:** you should create and use a unique event handler for each ad view.

To create the event handler you should provide a GAM Ad Unit Id and the list of available sizes for this ad unit.
**Note:** There is a helper function `convertGamAdSize` in GamBannerEventHandler to help you convert GAM AdSize into Apollo AdSize.


#### Step 2: Create Ad View

**BannerView** - is a view that will display the particular ad. It should be added to the UI. To create it you should provide:

- **configId** - an ID of Stored Impression on the Apollo server
- **eventHandler** - the instance of the banner event handler

Also, you should add the instance of `BannerView` to the UI.

And assign the [listeners](../android-in-app-bidding-listeners.md) for processing ad events.

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
    
    nativeAdConfiguration.nativeStylesCreative = nativeStylesCreative

    return nativeAdConfiguration
}
```
See more NativeAdConfiguration options [here](../native/android-native-ad-configuration.md).

#### Step 4: Load the Ad

Simply call the `loadAd()` method to start [In-App Bidding](../android-in-app-bidding-getting-started.md) flow.