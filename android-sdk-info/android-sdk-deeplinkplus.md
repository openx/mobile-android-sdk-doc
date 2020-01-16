Deep-Linking
================================================

OpenX SDK supports the premium standard for retargeting campaigns - [DeepLink+](https://developers.mopub.com/dsps/ad-formats/deep-linking/)

## Advantages

Processing traditional deep-links requires opening up blank browser windows, redirecting users multiple times, and sometimes breaking down completely. Additionally, buyers would lack analytics around when primary URLs worked versus when fallback URLs were required.

Deep Link+ provides a premium user experience while letting advertisers scale retargeting campaigns with accurate analytics. The new deeplinking format enables buyers to submit the following:

* primary URL
* fallback URL
* primary tracking URL
* fallback tracking URL.

And since Deep Link+ is built into the SDK, there is no need to pop up browser windows and re-directs that deteriorate the user experience.

## Support

The OpenX Mobile SDK supports the DeepLink+ starting from version **4.11**. The schema is supported for both kinds of ads - video and display.

The JSTag integration does not support it yet.


## How it works

DSPs should rely on the SDK version in the bid request:
```
"displaymanagerver": "4.11.0"
```

Starting with version 4.11.0 iOS SDK supports deeplink+.

To leverage the retargeting campaigns buyers use a specific scheme as click URL in the ad response. That URL describes the deep-linking and failover logic:

```
deeplink+://navigate?
    primaryUrl=PRIMARY_DEEPLINK&
    primaryTrackingUrl=PRIMARY_TRACKER&
    fallbackUrl=FALLBACK_URL
    &fallbackTrackingUrl=FALLBACK_TRACKER
```

The only required parameter is **`primaryUrl`** and if it’s the only one, it would be handled as existing deeplink URLs: idle if the app is missing.

The `fallbackUrl` can be any supported URI type (e.g., http, traditional deeplink) except for another Deep Link+ URL. To specify multiple tracker URLs (primary or fallback), buyers simply need to repeat the tracker key with any desired tracker URLs. The `primaryTrackingURL` is triggered if the deeplink is successful (which occurs after the user clicks).

For example, below is a Deep Link+ URL whose primary target is the Twitter app, with two (2) primary tracker URLs, a fallback URL directing the user to Twitter’s mobile website if the primary deeplink fails and zero (0) fallback URLs:

```
deeplink+://navigate?
    primaryUrl=twitter%3A%2F%2Ftimeline&
    primaryTrackingUrl=http%3A%2F%2Fmopub.com%2Fclicktracking&
    primaryTrackingUrl=http%3A%2F%2Fmopub.com%2Fmopubtracking&
    fallbackUrl=http%3A%2F%2Fmobile.twitter.com
```

The SDK will process this scheme regarding to the standard.

## Integration tips

**Publishers**: No action required for full-featured support of the ads with DeepLink+ schema. All work is performed by the SDK.

**Buyers**: Must insert the deeplink+ scheme into creative or provide it via redirect for the regular clickthrough URL.

## Demo

To inspect the behavior build and run OpenX Demo Application on the real device and try the examples:

- **Banner with deeplink+**
- **Video Interstitial with deeplink+**

You must also install the Twitter.

The display ad code sample:

```
<a href="deeplink+://navigate?primaryUrl=google%3A%2F%2Ftimeline&primaryTrackingUrl=http%3A%2F%2Fmopub.com%2Fclicktracking&primaryTrackingUrl=http%3A%2F%2Fmopub.com%2Fmopubtracking&fallbackUrl=http%3A%2F%2Fmobile.twitter.com&fallbackTrackingUrl=http%3A%2F%2Fmopub.com%2Fmopubtrackingfallback"><img width="320" height="50" src="https://ssl-i.cdn.openx.com/mobile/demo-creatives/mobile-demo-banner-640x100.png"></img></a>
```

The fragment of VAST file for video example:

```
<VideoClicks>
<ClickThrough>
<![CDATA[
deeplink+://navigate?primaryUrl=twitter%3A%2F%2Ftimeline&primaryTrackingUrl=http%3A%2F%2Fmopub.com%2Fclicktracking&primaryTrackingUrl=http%3A%2F%2Fmopub.com%2Fmopubtracking&fallbackUrl=http%3A%2F%2Fmobile.twitter.com&fallbackTrackingUrl=http%3A%2F%2Fmopub.com%2Fmopubtrackingfallback
]]>
</ClickThrough>
</VideoClicks>
```
