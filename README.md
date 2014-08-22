#MobUnity

Welcome to our solution to implement multiple ad networks, updating them on real time without any need to update your application again, and aim different ad network to different countries and boost your revenue. Visit http://mobunity.com to get more information.

####How can I implement this?
It's very easy. You will need to imagine and work with your application as you don't know which ad network you will use, but you know what kind of ad unit and which location you want. For example, onAppLaunch showing interstitial (let's say interstitial but it could be an offerwall, etc). Later from our dashboard you can configure which ad network you want to show onAppLaunch, and doesn't matter which one will be, it will fit.

####What options do I have?
Actually, we are working with the next ad networks
* AdBuddiz
* AdMob
* AppNext
* MobileCore
* RevMob
* StartApp

And with the next ad locations
* OnAppLaunch
* OnAppExit
* Banner
* Interstitial
* Extra

We will be adding more ad networks and ad locations, such as Video ads (Vungle, AdColony, etc).

####Show me some code!
It's very simple, to `initialize` mobunity, you will execute the next code in the first `Activity` inside of `onCreate`. It's static, that's why you need to initialize it only one time. It will request your configuration from the server and then be ready.

`MobUnity.init(Context context, String userId, String appId, MobUnityListener mobUnityListener)`

With `MobUnityListener` you will handle the initialization of MobUnity. You need to implement `onReady();` and `onServerError();`.

If `onServerError();` is executed, it means that something went bad, really bad, you may want to have a backup plan. It shoudln't happen, but we know how this works.

When `onReady();` gets called means that you can work with MobUnity and start loading the ads that you need.

Before starting to explain each of the positions, it's **very important** to know the workflow. When MobUnity it's initialized and `onReady();` gets callled, you **can't** then initialize all the ad positions. This is simple, some ad networks it's impossible to initialize more than one time because they are using `static` variables, so you wouldn't be able to, for example, use `MobileCore` in `OnAppLaunch`, `Interstitial` and `OnAppExit` at the same time. The workflow should look like this:
1. MobUnity's `onReady()` initialize (if it's needed) `OnAppLaunch`. When the interstitial of `OnAppLaunch` is `onClosed()`, then you can initialize the `Interstitial` and others like `Banner` and `Extra`.
2. If you want to show `OnAppExit`, you will `@Override` the `onBackPressed()`, and then initialize it and show it.

In the other hand, if you are **completely sure** that you will use different **ad network** on `Interstitial` and `OnAppExit`, then you can initialize also `OnAppExit` inside of `onAdClosed()` of `OnAppLaunch`.

###OnAppLaunch
* **Listener** `InterstitialListener`
  * `onAdLoaded(AdInterstitial ad);` ad is ready to use.
  * `onAdClosed();` ad clicked or closed.
  * `onNoConfigAvailable();` you didn't configure it in your dashboard.
* **How to initialize it?** `MobUnity.initAdOnAppLaunch(InterstitialListener interstitialListener)`
* **Style** Full screen ads (interstitial, offer wall, etc)

###OnAppExit
* **Listener** `InterstitialListener`
  * `onAdLoaded(AdInterstitial ad);` ad is ready to use.
  * `onAdClosed();` ad clicked or closed.
  * `onNoConfigAvailable();` you didn't configure it in your dashboard.
* **How to initialize it?** `MobUnity.initAdOnAppExit(InterstitialListener interstitialListener)`
* **Style** Full screen ads (interstitial, offer wall, etc)

###Interstitial
* **Listener** `InterstitialListener`
  * `onAdLoaded(AdInterstitial ad);` ad is ready to use.
  * `onAdClosed();` ad clicked or closed.
  * `onNoConfigAvailable();` you didn't configure it in your dashboard.
* **How to initialize it?** `MobUnity.initAdInterstitial(InterstitialListener interstitialListener)`
* **Style** Full screen ads (interstitial, offer wall, etc)

###Banner
* **Listener** `BannerListener`
  * `onAdLoaded(AdBanner ad);` ad is ready to use.
  * `onNoConfigAvailable();` you didn't configure it in your dashboard.
* **How to initialize it?** `MobUnity.initAdBanner(ViewGroup layout, BannerListener bannerListener)`. You will pass the `layout` where you want to attach the banner. It can be `LinearLayout`, `RelativeLayout` or any other.
* **Style** Banner.

###Extra
* **Listener** `ExtraListener`
  * `onAdLoaded(AdExtra ad);` ad is ready to use.
  * `onNoConfigAvailable();` you didn't configure it in your dashboard.
* **How to initialize it?** `MobUnity.initAdExtra(ExtraListener extraListener)`
* **Style** It can be Stickeez from `MobileCore` or the More Games/Apps button from `AppNext`
