# MoPub Setup

## Overview

The sense of prebid technology is to run the header bidding auction first and inject an opportunity to display a winning bid into a preconfigured waterfall on the Primary Ad Server. This could be achieved by adding special Line Items which point to the prebid creative. If such line item wins on the primary ad server the prebid ad will be rendered on the client, otherwise, the ad from the predefined waterfall will be rendered.

This scenario is totally supported by Apollo.

So the essential part of Apollo integration is creating a special Line Items on the MoPub.  

## Best Practises 

According to prebid's suggestions, you have to create a set of line items with unique price targets to get the best revenue. Line Items should be created according to the [price granularity](http://prebid.org/prebid-mobile/adops-price-granularity.html#autoGranularityBucket) policy. That means that you have to create more than one hundred line items to get the best coverage.

 
## Order Setup

### Manual

#### Step 1: Create New Order

 <img src="../res/orders/order-mopub-create.png" alt="Pipeline Screenshot" align="center">
 
### Step 2: Create Line Item
 
#### Line Item Type

To integrate the In-App Bidding into your app you have to create a Custom Ad Network Line Item with a specific Targeting keyword. Note that `Custom Ad Network` type is not suitable for Native Style Ads, see [Native Style Line Item and creative](#native-style-line-item-and-creative) for more details.

Regardless of the ability to name a Line Item in any way we strongly suggest using the price or targeting keyword in the name. It will help you when you will create a hundred of them.

- **Line Item Name**: hb_pb 0.10
- **Type & Priority**: Network Line Item
- **Network**: Custom SDK network
- **Custom event class**: 
    - For Banner API: **OpenXApolloBannerAdapter**
    - For Interstitial API: **OpenXApolloInterstitialAdapter**
    - For Rewarded API: **OpenXApolloRewardedVideoAdapter**
- **Custom event data**: {}

<img src="../res/orders/order-mopub-li-type.png" alt="Pipeline Screenshot" align="center">

#### Native Style Line Item and creative

Native styles ads are using `non-guaranteed` line item type and Medium Rectangle format HTML creative.

<img src="../res/orders/order-mopub-native-ad-li.png" alt="Pipeline Screenshot" align="center">

MoPub 300x250 Medium Rectangle format HTML creative example:

<img src="../res/orders/order-mopub-native-ad-creative.png" alt="Pipeline Screenshot" align="center">

``` html
<div class="sponsored-post">
  <div class="thumbnail"></div>
  <div class="content" class="pb-click">
	<h2><p><span style="display:inline-block;"><img src="hb_native_icon" alt="hb_native_icon" width="40" height="40"></span> hb_native_title</p></h2>
	<p>hb_native_body</p>
	<a target="_blank" href="hb_native_linkurl" class="pb-click">hb_native_cta</a>
	<img src="hb_native_image" alt="hb_native_image" width="300" height="50">
	<div class="attribution">hb_native_brand</div>
  </div>
</div>
<script src="https://cdn.jsdelivr.net/npm/prebid-universal-creative@latest/dist/native-trk.js"></script>
<script>
  var pbNativeTagData = {};
  pbNativeTagData.uuid = "%%KEYWORD:hb_cache_id%%";
  pbNativeTagData.env = "%%KEYWORD:hb_env%%";
  pbNativeTagData.cacheHost = "%%KEYWORD:hb_cache_host%%";
  pbNativeTagData.cachePath = "%%KEYWORD:hb_cache_path%%";
  window.pbNativeTag.startTrackers(pbNativeTagData);
</script>
```
 
#### Ad Unit Targeting

<img src="../res/orders/order-mopub-li-ad-unit.png" alt="Pipeline Screenshot" align="center">

#### Audience Targeting

The **Keyword targeting** property should contain a special keyword with the price of winning bid.

<img src="../res/orders/order-mopub-li-audience.png" alt="Pipeline Screenshot" align="center">


