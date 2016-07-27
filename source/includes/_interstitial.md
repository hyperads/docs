# Interstitial ads

## Mobile Web

* Go to the Publisher UI
* Create new App
* Create new Placement for it
* On placements list click on Tag & SDK and select appropriate integration.

## iOS

The HyperAdX Interstitial ads allows you to monetize your iOS apps with banner ads. This guide explains how to add banner ads to your app. If you're interested in other kinds of ad units, see the list of available types.

### Set up the SDK

**Manual**

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxiOSADs_Sample_v2.0.0.zip) and extract the HADFramework for iOS.
* Open your project target General tab.
* Drag the HADFramework.framework file to Embedded Binaries.
* Open your project target Build Settings tab. (Required only for Objective-C projects)
* Set "Embedded Content Contains Swift Code" to Yes. (Required only for Objective-C projects)
* Add the AdSupport framework to your project.

### Swift implementation

> First of all, in your AppDelegate file create an instance of HADFramework
> Import the SDK

```swift
import HADFramework
```

> And in your application didFinishLaunchingWithOptions method call HAD.create()

```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
  // Override point for customization after application launch.
  HAD.create()
  return true
}
```

> Ok, let's move to your View Controller. Import the SDK, declare that you implement the HADInterstitialDelegate protocol and add an instance variable for the interstitial ad unit:

```swift
import UIKit
import HADFramework

class MyViewController: UIViewController, HADInterstitialDelegate {

}
```

> Add a function in your View Controller that initializes the interstitial view. You will typically call this function ahead of the time you want to show the ad:

```swift
override func viewDidLoad() {
  super.viewDidLoad()
  loadInterstitalAd()
}

func loadInterstitalAd() {
  let interstitial = HADInterstitial(placementId: "PLACEMENT_ID")
  interstitial.delegate = self
  interstitial.loadAd()
}
```

> Now that you have added the code to load the ad, add the following functions to handle loading failures and to display the ad once it has loaded:

```swift
//MARK: HADInterstitial Delegate

func HADInterstitialDidLoad(controller: HADInterstitial) {
  controller.modalTransitionStyle = .CoverVertical
  controller.modalPresentationStyle = .FullScreen
  presentViewController(controller, animated: true, completion: nil)
}

func HADInterstitialDidFail(controller: HADInterstitial, error: NSError?) {
  print("HADInterstitialDidFail: \(error)")
}
```

> Optionally, you can add the following functions to handle the cases where the full screen ad is closed or when the user clicks on it:

```swift
func HADInterstitialDidClick(controller: HADInterstitial) {
  print("HADInterstitialDidClick")
}

func HADInterstitialWillClose(controller: HADInterstitial) {
  print("HADInterstitialWillClose")
}

func HADInterstitialDidClose(controller: HADInterstitial) {
  print("HADInterstitialDidClose")
}
```

### Objective-C implementation

> First of all, in your AppDelegate file create an instance of HADFramework
> Import the SDK header in AppDelegate.h:

```objective_c
#import <HADFramework/HADFramework.h>
```

> And in your application didFinishLaunchingWithOptions method call [HAD create]

```objective_c
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  // Override point for customization after application launch.
  [HAD create];
  return YES;
}
```

> Ok, let's move to your View Controller. In implementation file, import the SDK header, declare that you implement the HADInterstitialDelegate protocol and add an instance variable for the interstitial ad unit:

```objective_c
#import <UIKit/UIKit.h>
#import <HADFramework/HADFramework.h>

@interface MyViewController () <HADInterstitialDelegate>
@property (strong, nonatomic) HADInterstitial *interstitial;
@end
```

> Add a function in your View Controller that initializes the interstitial view. You will typically call this function ahead of the time you want to show the ad:

```objective_c
- (void) viewDidLoad {
  [super viewDidLoad];

  [self loadInterstitialAd];
}

- (void) loadInterstitalAd {
  self.interstitial = [[HADInterstitial alloc] initWithPlacementId:@"PLACEMENT_ID"];
  self.interstitial.delegate = self;
  [self.interstitial loadAd];
}
```

> Now that you have added the code to load the ad, add the following functions to handle loading failures and to display the ad once it has loaded:

```objective_c
#pragma mark - HADInterstitialDelegate

-(void)HADInterstitialDidLoad:(HADInterstitial *)controller {
  NSLog(@"HADInterstitialDidLoad");
  self.interstitial.modalPresentationStyle = UIModalPresentationFullScreen;
  self.interstitial.modalTransitionStyle = UIModalTransitionStyleCoverVertical;
  [self presentViewController:self.interstitial animated:YES completion:nil];
}

-(void)HADInterstitialDidFail:(HADInterstitial *)controller error:(NSError *)error {
  NSLog(@"HADInterstitialDidFail: %@", error);
}
```

> Optionally, you can add the following functions to handle the cases where the full screen ad is closed or when the user clicks on it:

```objective_c
-(void)HADInterstitialDidClick:(HADInterstitial *)controller {
  NSLog(@"HADInterstitialDidClick");
}

-(void)HADInterstitialWillClose:(HADInterstitial *)controller {
  NSLog(@"HADInterstitialWillClose");
}

-(void)HADInterstitialDidClose:(HADInterstitial *)controller {
  NSLog(@"HADInterstitialDidClose");
}
```

###Admob Adapter

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxiOSAdmobAdapter_2.0.0.zip) and extract the AdMob adapter if needed.

* First of all you need to add new app in AdMob console.

* You will get UnitId string like 'ca-app-pub-*************/*************'.
For the next few hours you may get the AdMob errors with codes 0 or 2. Just be patient.

Then you need to add new mediation source.

* Fill `Class Name` field with a `HADCustomEventInterstitial`. And a `Parameter` with your HyperAdx statement string.

* Setup eCPM for new network

Now you can setting up your Xcode project.

* Put HyperAdx-SDK as described above
* Add HADCustomEventInterstitial.swift file

**NOTE** - In the Objective-C only project you must create swift header file as described here e.g. http://stackoverflow.com/questions/24102104/how-to-import-swift-code-to-objective-c

> Just create AdMob interstitial Ad as usually:

```swift
import GoogleMobileAds
import HADFramework
import UIKit

class ViewController: UIViewController, GADInterstitialDelegate {
    var interstitial: GADInterstitial!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        let request = GADRequest()
        //Interstitial
        interstitial = GADInterstitial(adUnitID: "YOUR_ADUNIT_ID")
        interstitial.delegate = self
        interstitial.loadRequest(request)
    }
    
    @IBAction func createAndLoadInterstitial() {
        if interstitial.isReady {
            interstitial.presentFromRootViewController(self)
        } else {
            print("Ad wasn't ready")
        }
    }
    
    //MARK: GADInterstitialDelegate
    func interstitialDidReceiveAd(ad: GADInterstitial!) {
        print("interstitialDidReceiveAd")
    }
    
    func interstitial(ad: GADInterstitial!, didFailToReceiveAdWithError error: GADRequestError!) {
        print("interstitial didFailToReceiveAdWithError: \(error)")
    }
}
```

###Mopub Adapter

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxiOSMoPubAdapter_2.0.0.zip) and extract the Mopub adapter if needed.

You can use Hyperadx as a Network in Mopub's Mediation platform.

Setup SDKs:

* Integrate with Mopub SDK (https://github.com/mopub/mopub-ios-sdk/wiki/Manual-Native-Ads-Integration-for-iOS)
* Install Hyperadx SDK
* Add HADInterstitialCustomEvent.swift file

**NOTE** - In the Objective-C only project you must create swift header file as described here e.g. http://stackoverflow.com/questions/24102104/how-to-import-swift-code-to-objective-c

Setup Mopub Dashboard

* Create an "Hyperadx" Network in Mopub's dashboard and connect it to your Ad Units.
* In Mopub's dashboard select Orders > Add a New Order
* This screen shows forms for creating Order and Line item at the same time
* The most interested part is "Line Item Details"
* Choose Network > Custom Native Network
* Fill Class field with `HADInterstitialCustomEvent`
* Fill Data field with: `{"placementId": "<YOUR PLACEMENT>"}`
* In "Ad Unit Targeting" section select ad units with native format
* In Mopub's dashboard select Networks > Add New network
* Then select Custom Native Network
* Complete the fields accordingly to the Ad Unit that you want to use
* Create new Order in Orders tab
* Complete the fields accordingly to the Ad Unit that you want to use

Custom Event Class: `HADInterstitialCustomEvent`

Custom Event Class Data: `{"PLACEMENT":"<YOUR PLACEMENT>"}`

You can use the test placement `5b3QbMRQ`

> Add `HADInterstitialCustomEvent.swift` adapter in your project
Implement MoPub Interstitial:

```swift
import HADFramework
import UIKit

class ViewController: UIViewController, MPInterstitialAdControllerDelegate {
    var interstitial: MPInterstitialAdController!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        //Interstitial
        interstitial = MPInterstitialAdController(forAdUnitId: "YOUR_AD_UNIT")
        interstitial.delegate = self
        interstitial.loadAd()
    }
    
    //MARK: MPInterstitialAdControllerDelegate
    func interstitialDidLoadAd(interstitial: MPInterstitialAdController!) {
        print("interstitialDidLoadAd")
        interstitial.showFromViewController(self)
    }
    
    func interstitialDidFailToLoadAd(inter: MPInterstitialAdController!) {
        print("interstitialDidFailToLoadAd")
        interstitial = MPInterstitialAdController(forAdUnitId: "5b0f8ff979a840b4928ca7fd14ec82e7")
        interstitial.delegate = self
        interstitial.loadAd()
    }
}
```

> This is your Hyperadx Interstitial MoPub adapter. Now you can use Mopub as usual.

## Android

The HyperAdX Interstitial ads allows you to monetize your Android apps with banner ads. This guide explains how to add banner ads to your app. If you’re interested in other kinds of ad units, see the list of available types.

Sample projects:

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidADs_Sample_v1.2.4.zip) and extract the Example app for Android.

### Set up the SDK

> Add following under manifest tag to your AndroidManifest.xml:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

> Put the HyperAdxSDK_xxx.jar in “libs” folder in your Android Studio or Eclipse. Add it to dependencies in build.grandle file. Also you need to add google play services.

```groove
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile 'com.android.support:appcompat-v7:23.4.0'
    compile 'com.android.support:support-v4:23.4.0'
    compile 'com.google.android.gms:play-services-ads:9.0.2'
    compile 'com.google.android.gms:play-services-base:9.0.2'
}
```

> Then, create a function that requests a interstitial ad. The SDK will log the impression and handle the click automatically.

```java
private void loadInterstitialAd() {
        interstitialAd = new HADInterstitialAd(this /*Strongly recomend to use Activity context*/,
                getString(R.string.Placement)); //Interstitial AD constructor.

        interstitialAd.setAdListener(new InterstitialAdListener() { // Set Listener
            @Override
            public void onAdLoaded(com.hyperadx.lib.sdk.interstitialads.Ad ad) { // Called when AD is Loaded
                iAd = ad;
                Toast.makeText(MainActivity.this, "Interstitial Ad loaded", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onError(com.hyperadx.lib.sdk.interstitialads.Ad Ad, String error) { // Called when load is fail
                Toast.makeText(MainActivity.this, "Interstitial Ad failed to load with error: " + error, Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onInterstitialDisplayed() { // Called when Ad was impressed
                Toast.makeText(MainActivity.this, "Tracked Interstitial Ad impression", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onInterstitialDismissed(com.hyperadx.lib.sdk.interstitialads.Ad ad) { // Called when Ad was dissnissed by user
                Toast.makeText(MainActivity.this, "Interstitial Ad Dismissed", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdClicked() { // Called when user click on AD
                Toast.makeText(MainActivity.this, "Tracked Interstitial Ad click", Toast.LENGTH_SHORT).show();

            }
        });
        interstitialAd.loadAd(); // Call to load AD
    }

public void showInterstitial(View view) {
    if (iAd != null)
        InterstitialAd.show(iAd); // Call to show AD
    else
        Toast.makeText(this, "The Interstitial AD not ready yet. Try again!", Toast.LENGTH_LONG).show();
}
```

###Admob Adapter

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidAdmobSample_1.0.1.zip) and extract the AdMob adapter if needed.

* First of all you need to add new app in AdMob console.

* You will get UnitId string like 'ca-app-pub-*************/*************'.
For the next few hours you may get the AdMob errors with codes 0 or 2. Just be patient.

Then you need to add new mediation source.

* Fill `Class Name` field with a `com.hyperadx.admob.HADInterstitialEvent` string. And a `Parameter` with your HyperAdx statement string.

* Setup eCPM for new network

Now you can setting up your android project.

* Put HyperAdx-SDK and AdMob-adapter in 'libs' folder.
* Add those lines in your `build.gradle` file:

**NOTE** - Admob interstitial may not work in emulator. Use real devices even for tests!

```groove
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.android.support:appcompat-v7:23.4.0'
    compile 'com.google.android.gms:play-services-ads:9.0.2'
    compile 'com.google.android.gms:play-services-base:9.0.2'
}
```
> Just create AdMob interstitial Ad as usually:

```java
package com.hyperadx.admob_sample;
import android.os.Bundle;
import android.support.v7.app.AppCompatActivity;
import android.view.View;
import android.widget.Toast;
import com.google.android.gms.ads.AdRequest;
public class MainActivity extends AppCompatActivity {
    private com.google.android.gms.ads.InterstitialAd mAdapterInterstitial;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        loadInterstitialAd();
    }

    private void loadInterstitialAd() {
        mAdapterInterstitial = new com.google.android.gms.ads.InterstitialAd(this);
        mAdapterInterstitial.setAdUnitId(
                "ca-app-pub-6172762133617463/5529648238"
        );

        mAdapterInterstitial.setAdListener(new com.google.android.gms.ads.AdListener() {
            @Override
            public void onAdFailedToLoad(int errorCode) {
                Toast.makeText(MainActivity.this,
                        "Error loading adapter interstitial, code " + errorCode,
                        Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdLoaded() {
                Toast.makeText(MainActivity.this,
                        "onAdLoaded()",
                        Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onAdOpened() {
            }

            @Override
            public void onAdClosed() {
                mAdapterInterstitial.loadAd(new AdRequest.Builder().build());
            }
        });

        mAdapterInterstitial.loadAd(new AdRequest.Builder().build());
    }


    public void showInterstitial(View view) {
        if (mAdapterInterstitial.isLoaded())
            mAdapterInterstitial.show();
        else
            Toast.makeText(this, "The Interstitial AD not ready yet. Try again!", Toast.LENGTH_LONG).show();
    }
}
```

###Mopub Adapter

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidMoPubAdapter_1.1.0.zip) and extract the Mopub adapter if needed.

You can use Hyperadx as a Network in Mopub's Mediation platform.

Setup SDKs:

* Integrate with Mopub SDK (https://github.com/mopub/mopub-android-sdk/wiki/Getting-Started)
* Install Hyperadx SDK

Setup Mopub Dashboard

* Create an "Hyperadx" Network in Mopub's dashboard and connect it to your Ad Units.
* In Mopub's dashboard select Networks > Add New network
* Then select Custom Native Network
* Complete the fields accordingly to the Ad Unit that you want to use


Custom Event Class: `com.mopub.mobileads.HyperadxInterstitialMopub`

Custom Event Class Data: `{"PLACEMENT":"<YOUR PLACEMENT>"}`

You can use the test placement `5b3QbMRQ`

> Add adapter in your project
Create package "com.mopub.mobileads" in your project and put this class in there:

```java
HyperadxInterstitialMopub.java:

package com.mopub.mobileads;
import android.content.Context;
import android.util.Log;
import com.hyperadx.lib.sdk.interstitialads.HADInterstitialAd;
import com.hyperadx.lib.sdk.interstitialads.InterstitialAdListener;
import java.util.Map;

public class HyperadxInterstitialMopub extends CustomEventInterstitial {
    private static final String PLACEMENT_KEY = "PLACEMENT";
    private HADInterstitialAd interstitialAd;
    private com.hyperadx.lib.sdk.interstitialads.Ad iAd = null;
    CustomEventInterstitialListener customEventInterstitialListener;

    @Override
    protected void loadInterstitial(final Context context, final CustomEventInterstitialListener customEventInterstitialListener, Map<String, Object> localExtras, Map<String, String> serverExtras) {
        final String placement;
        final String appId;
        if (serverExtras != null && serverExtras.containsKey(PLACEMENT_KEY)) {
            placement = serverExtras.get(PLACEMENT_KEY);

 } else {
            customEventInterstitialListener.onInterstitialFailed(MoPubErrorCode.ADAPTER_CONFIGURATION_ERROR);
            return;
        }

        interstitialAd = new HADInterstitialAd(context, placement); //Interstitial AD constructor
        interstitialAd.setAdListener(new InterstitialAdListener() { // Set Listener
            @Override
            public void onAdLoaded(com.hyperadx.lib.sdk.interstitialads.Ad ad) { // Called when AD is Loaded
                iAd = ad;
                //   Toast.makeText(context, "Interstitial Ad loaded", Toast.LENGTH_SHORT).show();
                customEventInterstitialListener.onInterstitialLoaded();
            }

            @Override
            public void onError(com.hyperadx.lib.sdk.interstitialads.Ad Ad, String error) { // Called when load is fail
                //   Toast.makeText(context, "Interstitial Ad failed to load with error: " + error, Toast.LENGTH_SHORT).show();
                customEventInterstitialListener.onInterstitialFailed(MoPubErrorCode.UNSPECIFIED);

            @Override
            public void onInterstitialDisplayed() { // Called when Ad was impressed
                //    Toast.makeText(context, "Tracked Interstitial Ad impression", Toast.LENGTH_SHORT).show();
                customEventInterstitialListener.onInterstitialShown();
            }

            @Override
            public void onInterstitialDismissed(com.hyperadx.lib.sdk.interstitialads.Ad ad) { // Called when Ad was dissnissed by user
                //   Toast.makeText(context, "Interstitial Ad Dismissed", Toast.LENGTH_SHORT).show();
                customEventInterstitialListener.onInterstitialDismissed();

            @Override
            public void onAdClicked() { // Called when user click on AD
                //   Toast.makeText(context, "Tracked Interstitial Ad click", Toast.LENGTH_SHORT).show();
                customEventInterstitialListener.onInterstitialClicked();
            }
        });

        this.customEventInterstitialListener = customEventInterstitialListener;
        interstitialAd.loadAd(); // Call to load AD

    @Override
    protected void showInterstitial() {
        if (iAd != null)
            HADInterstitialAd.show(iAd); // Call to show AD
        else
            Log.e("HADInterstitialMopub", "The Interstitial AD not ready yet. Try again!");
    }

    @Override
    protected void onInvalidate() {
    }
}
```

> This is your adapter. Now you can use Mopub as usual.
