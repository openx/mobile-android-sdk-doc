Impression Tracking
=========================================


## Creative's Pixels

Starting from 4.11 OpenX SDK tracks the impression pixel according to definition of **render impression** from [Mobile Application Advertising Measurement Guidelines](http://mediaratingcouncil.org/Mobile%20In-App%20Measurement%20Guidelines%20(MMTF%20Final%20v1.1).pdf):


> **Ad Impression**: A measurement of responses from an ad delivery system to an ad request from the user's device, which is filtered for invalid traffic and is recorded at a point as late as possible in the process of delivery of the creative material to the user's device. The ad must be loaded and at minimum begin to render in order to count it as a valid ad impression. Measurement of begin to render should include logical components necessary to display the ad, but does not necessarily include logical elements that are not essential (such as other tracking elements).
>
> In the context of the guidance above, “loaded” means the logical creative file has been transmitted and received at the client-side (user device) and “render” refers to the process of painting the creative file or adding it to any portion of the Document Object Model.

The impression pixel is triggered when creative appears on display, at least with 1 pixel.
This rule is applied to all tracking pixels, which managed by OpenX SDK - display, video, Open Measurement.

## MRAID

The OpenX SDK supports the MRAID 2.0. SDK broadcasts the **`mraid.viewableChange()`** event when the ad becomes rendered. It means that for proper impression tracking with MRAID the creative's code for tracking impression must depend on **`mraid.isViewable()`**. For example:


``` javascript
if ( mraid.viewableChangeEventWasDetected() )
   if( mraid.isViewable() == true)
         fireMyImpressionTrackers();
   else if ( mraid.isViewable() == false)
         doNothing();
```

Otherwise the impression tracking would be inconsistent with OpenX approach.
