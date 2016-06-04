# Interstitial ads

## iOS

The HyperAdX Interstitial ads allows you to monetize your iOS apps with banner ads. This guide explains how to add banner ads to your app. If you're interested in other kinds of ad units, see the list of available types.

### Set up the SDK

**Manual**

* Download and extract the HADFramework for iOS.
* Drag the HADFramework.framework file to your Xcode project and place it under the Frameworks folder.
* Add the AdSupport framework to your project.

### Implementation

> Now, in your View Controller implementation file, import the SDK header, declare that you implement the HADInterstitialDelegate protocol and add an instance variable for the interstitial ad unit:

```objective_c
#import <UIKit/UIKit.h>
#import <HADFramework/HADInterstitial.h>

@interface MyViewController : UIViewController<HADInterstitialDelegate>
  @property (nonatomic, strong) HADInterstitial* interstitial;
@end
```

> Add a function in your View Controller that initializes the interstitial view. You will typically call this function ahead of the time you want to show the ad:

```objective_c
- (void) viewDidLoad {
  [super viewDidLoad];

  [self loadInterstitialAd];
}

- (void) loadInterstitalAd {
  self.interstitial = [[HADInterstitial alloc] initWithPlacementId:PLACEMENT_ID delegate:self];
  [self.interstitial loadAd];
}
```

> Now that you have added the code to load the ad, add the following functions to handle loading failures and to display the ad once it has loaded:

```objective_c
- (void) HADInterstitialDidLoad:(HADInterstitial*)controller {
  [controller showFromController:self];
}

- (void) HADInterstitial:(HADInterstitial*)controller didFailWithError:(NSError*)error {
  NSLog(@"ERROR: %@",error.localizedDescription);
}

- (void) HADInterstitialDidClick:(HADInterstitial*)controller {
  NSLog(@"CLICKED AD");
}
```

> Optionally, you can add the following functions to handle the cases where the full screen ad is closed or when the user clicks on it:

```objective_c
- (void) HADInterstitialDidClose:(HADInterstitial*)controller {
  // Consider to add code here to resume your app's flow
}

- (void) HADInterstitialWillClose:(HADInterstitial*)controller {
  // Consider to add code here to resume your app's flow
}
```

## Android

The HyperAdX Interstitial ads allows you to monetize your Android apps with banner ads. This guide explains how to add banner ads to your app. If you’re interested in other kinds of ad units, see the list of available types.

### Set up the SDK

Follow these steps to download and include it in your project:

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidADs_Sample_v1.2.2.zip) and extract the Example app for Android.
* Open pdf file from archive for instructions.
* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidAdmobSample_1.0.1.zip) and extract the AdMob adapter if needed.
* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidMoPubAdapter_1.1.0.zip) and extract the Mopub adapter if needed.
