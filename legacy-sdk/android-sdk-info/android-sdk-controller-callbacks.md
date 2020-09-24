Callbacks
=========


Using the BannerViewListener interface
----------------------------

You can assign an ad event listener callback to receive events via the
`bannerView.addAdEventListener()` method. Using these callbacks is highly
recommended for understanding the events in your app.


| **Callback method**                               | **Invoked when**                                             |
| ------------------------------------------------- | ------------------------------------------------------------ |
| adDidLoad(BannerView bannerView, AdDetails adDetails)     | An ad is loaded though not necessarily shown.<br />**Note:**  `AdDetails` includes a `transactionId` that uniquely identifies the ad, which you can use for managing and reporting ad quality issues. |
| adDidFailToLoad(BannerView bannerView, AdException error) | The loading of an ad fails for any reason. For details about the types of exceptions that can occur, see [Ad exceptions](android-sdk-self-test.md#ad-exception-types-and-examples). |
| adDidDisplay(BannerView bannerView)                       | A loaded ad is displayed.                                    |
| adDidComplete(BannerView bannerView)                      | An ad has finished displaying.                               |
| adWasClicked(BannerView bannerView)                       | An ad was clicked.                                           |
| adClickThroughDidClose(BannerView bannerView)             | A clicked ad was closed.                                     |
| adDidExpand(BannerView bannerView)                        | An MRAID banner ad is expanded.                              |
| adDidCollapse(BannerView bannerView)                      | An expanded MRAID banner ad is collapsed.                    |

Using the InterstitialViewListener interface
----------------------------

You can assign an ad event listener callback to receive events via the
`interstitialView.addAdEventListener()` method. Using these callbacks is highly
recommended for understanding the events in your app.


| **Callback method**                               | **Invoked when**                                             |
| ------------------------------------------------- | ------------------------------------------------------------ |
| adDidLoad(InterstitialView interstitialView, AdDetails adDetails)     | An ad is loaded though not necessarily shown.<br />**Note:**  `AdDetails` includes a `transactionId` that uniquely identifies the ad, which you can use for managing and reporting ad quality issues. |
| adDidFailToLoad(InterstitialView interstitialView, AdException error) | The loading of an ad fails for any reason. For details about the types of exceptions that can occur, see [Ad exceptions](android-sdk-self-test.md#ad-exception-types-and-examples). |
| adDidDisplay(InterstitialView interstitialView)                       | A loaded ad is displayed.                                    |
| adDidComplete(InterstitialView interstitialView)                      | An ad has finished displaying.                               |
| adWasClicked(InterstitialView interstitialView)                       | An ad was clicked.                                           |
| adClickThroughDidClose(InterstitialView interstitialView)             | A clicked ad was closed.                                     |
| adInterstitialDidClose(InterstitialView interstitialView)             | An interstitial ad was closed by the user.                       |

Using the VideoAdViewListener interface
----------------------------

You can assign an ad event listener callback to receive events via the
`videoAdView.addListener()` method. Using these callbacks is highly
recommended for understanding the events in your app.


| **Callback method**                               | **Invoked when**                                             |
| ------------------------------------------------- | ------------------------------------------------------------ |
| adDidLoad(VideoAdView videoAdView, AdDetails adDetails)     | An ad is loaded though not necessarily shown.<br />**Note:**  `AdDetails` includes a `transactionId` that uniquely identifies the ad, which you can use for managing and reporting ad quality issues. |
| adDidFailToLoad(VideoAdView videoAdView, AdException error) | The loading of an ad fails for any reason. For details about the types of exceptions that can occur, see [Ad exceptions](android-sdk-self-test.md#ad-exception-types-and-examples). |
| adDidDisplay(VideoAdView videoAdView)| A loaded ad is displayed.|
| adDidComplete(VideoAdView videoAdView)| An ad has finished displaying.|
| adInterstitialDidOpen(VideoAdView videoAdView)| Ad is opened in the interstitial mode and continue playing.|
| adClickthroughDidOpen(VideoAdView videoAdView) | The user opens a clickthrough for the ad.|
| adClickthroughDidClose(VideoAdView videoAdView)| The user has closed an interstitial.|
| adDidLeaveApplication(VideoAdView videoAdView) | Ad causes the SDK to leave the app.|
