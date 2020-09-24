Ad quality
==========

The OpenX Mobile Android SDK leverages the server-side controls on the
OpenX platform to safeguard apps against low quality ads, including a
creative scanner, whitelist and blacklist capabilities, and brand
filters. The SDK provides smart logic that minimizes re-directs and
spoofed user clicks. It also exposes a unique transaction ID for every
banner and interstitial display ad, giving you immediate control to
manage and report ad quality issues as they occur.

Transaction ID for identifying ads
----------------------------------

The [callback method](android-sdk-controller-callbacks.md) `adDidLoad`
returns `AdDetails`, which includes a `transactionId` that uniquely
identifies the ad. Use the `adDidLoad` method in your `AdEventsListener`
interface to always have a unique identifier for each ad delivered by
the SDK. See examples in [banner
integration](android-sdk-banner-integration.md#using-the-listeneriinterface) and
[interstitial
integration](android-sdk-interstitial-integration.md#using-adeventslistener).

Reporting ad quality issues
---------------------------

If you need to report an ad to OpenX, please create an Ad Quality case
through the [OpenX Support Community](https://community.openx.com/s/)
and include as much of the following information as possible:
-   The transaction ID.
-   Screenshots and device information.
-   Category of the ad quality issue. See the list of categories available in the table below.
  
  
      |Reason                    |  Description|
      |--------------------------| ------------|
      |Adult/Suggestive          |  Ad contains adult or suggestive images or themes.|
      |Audio or Video: Auto-Play |  Ad automatically plays audio or video without user interaction.|
      |Blanks                    |  Ad unit consistently renders a blank.|
      |Brand Violation           |  Ad violates my filter settings.|
      |Malware                   |  Ad attempts to install malware on the user\'s machine.|
      |Misplaced Ad/Error        |  Ad renders outside of the ad unit or in an unexpected way.|
      |Pop Ups/Under             |  Ad causes a pop-up or pop-under window to appear on the user\'s screen.|
      |Redirect                  |  Ad causes an automatic redirect to a new webpage or app store page.|
      |Sensitive Category        |  Ad contains objectionable images or themes.|
      |Other                     |  Use this option to report other ad quality issue.|
    

OpenX will review the trace information about the ad and work to resolve
the issue.

> **Note:** Transaction IDs are currently not supported for video interstitial ads.
