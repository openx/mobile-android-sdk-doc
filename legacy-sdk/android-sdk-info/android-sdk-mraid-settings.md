MRAID settings
==============

OpenX Mobile Android SDK allows you to disable some Mobile Rich Media Ad
Interface (MRAID) function calls. For example, you can prevent an MRAID
ad from performing actions such as making phone calls or sending SMS
messages. For details about what you can disable, see the Access to
Native Features section of the [MRAID 2.0
specification](http://www.iab.net/media/file/IAB_MRAID_v2_FINAL.pdf).

To disable an MRAID function, add the following code to your project
before creating an ad:

``` java
OXSettings.setDisabledSupportFlags(OXSettings.TEL | OXSettings.CALENDAR);
```

To disable multiple functions, separate desired function flags with the
pipe symbol ( \| ). Use the following flags to disable the
corresponding functionality:

-   `OXSettings.CALENDAR`
-   `OXSettings.EMAIL`
-   `OXSettings.INLINE\_VIDEO`
-   `OXSettings.SMS`
-   `OXSettings.STORE\_PICTURE`
-   `OXSettings.TEL`

> **Note:** The changes are reflected in all subsequent ad calls and need to be set
only once.
