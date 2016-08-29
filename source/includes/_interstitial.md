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

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxiOSADs_Sample_v2.0.2.zip) and extract the HADFramework for iOS.
* Open your project target General tab.
* Drag the HADFramework.framework file to Embedded Binaries.
* Open your project target Build Settings tab. (Required only for Objective-C projects)
* Set "Embedded Content Contains Swift Code" to Yes. (Required only for Objective-C projects)
* Add the AdSupport framework to your project.

### iOS 7

If you want to support iOS7 - [download](https://s3-us-west-2.amazonaws.com/adpanel-public/HADFramework-ObjC-iOS7%2B.framework.zip) our legacy SDK. It supports only NativeAds.

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

## Android

The HyperAdX Interstitial ads allows you to monetize your Android apps with banner ads. This guide explains how to add banner ads to your app. If you’re interested in other kinds of ad units, see the list of available types.

Sample projects:

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidADs_Sample_v1.2.7.zip) and extract the Example app for Android.

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
