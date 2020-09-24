Flex ads
========

The OpenX user interface supports choosing multiple ad sizes for an ad
unit. By default, the setting in the UI will result in ad delivery of
any one of these sizes based on best possibility of monetization. If you
want to specify a subset of your ad units\' supported ad sizes at
delivery time, such as flexible sizes only for certain refreshes, use
the `setFlexAdSize` parameter:

``` java
 /* Set predefined flex adsize here. Please refer 'AdConfiguration.OXMAdSize' for more OpenX predefined flex ad sizes. */
adView.setFlexAdSize(AdConfiguration.OXMAdSize.BANNER_320x50);
 
// Or, set your custom flex adsize as a string.
adView.setFlexAdSize("728x90,320x50");
```

Note that this approach does not apply to video interstitial ads,
because VAST specifications already support multiple media files.

Enabling multiple ad sizes for an ad unit (flexible ad sizes) allows for
better monetization because more ad sizes can fill an impression, but it
also means that in some cases, a smaller ad may fill. This could create
more blank space. If user experience is important, you may prefer to use
a single ad size.

If you do not set these values programmatically, then the values set in
the UI will be used for bid requests.

The supported ad sizes are as follows:

 
``` java
AdConfiguration.OXMAdSize.BANNER_320x50

AdConfiguration.OXMAdSize.BANNER_300x250

AdConfiguration.OXMAdSize.BANNER_320x50_300x250

AdConfiguration.OXMAdSize.INTERSTITIAL_320x480

AdConfiguration.OXMAdSize.INTERSTITIAL_300x250

AdConfiguration.OXMAdSize.INTERSTITIAL_480x320

AdConfiguration.OXMAdSize.INTERSTITIAL_768x1024

AdConfiguration.OXMAdSize.INTERSTITIAL_1024x768

 

// Flexible ad sizes for portrait, phone

AdConfiguration.OXMAdSize.INTERSTITIAL_320x480_300x250

 

// Flexible ad sizes for landscape, phone

AdConfiguration.OXMAdSize.INTERSTITIAL_480x320_300x250

 

// Flexible ad sizes for portrait, tablet

AdConfiguration.OXMAdSize.INTERSTITIAL_768x1024_320x480_300x250

 

// Flexible ad sizes for landscape, tablet

AdConfiguration.OXMAdSize.INTERSTITIAL_1024x768_480x320_300x250
```