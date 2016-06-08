# Native ads

HyperAdX uses Placement ID to allow access to the API. You can register a new App and create Placement at our [developer portal](http://hyperadx.com/publishers/sign_in).

HyperAdX expects for Placement ID to be included in all API requests to the server in a get variable that looks like the following:

<aside class="notice">
You must replace <code>PLACEMENT_ID</code> with your placement's ID.
</aside>

## Native Ads in Mobile Web

* Go to the Publisher UI
* Create new App
* Create new Placement for it
* On placements list click on Tag & SDK and select appropriate integration.

## Native Ads in iOS

The HyperAdX's Native Ads allows you to build a customized experience for the ads you show in your app. When using the Native Ad API, instead of receiving an ad ready to be displayed, you will receive a group of ad properties such as a title, an image, a call to action, and you will have to use them to construct a custom UIView where the ad is shown.

**There are three actions you will need to take to implement this in your app:**

* Request an ad
* Use the returned ad metadata to build a custom native UI
* Register the ad's view with the `HADNativeAd` instance

### Set up the SDK

Follow these steps to download and include it in your project:

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/TestADFramework-1.1.0.zip) and extract the Example app for iOS.
* Drag the HADFramework.framework file to your Xcode project and place it under the Frameworks folder.
* Add the AdSupport framework to your project.

### Implementation

> Now, in your View Controller implementation file, import the SDK  and declare that you implement the `HADNativeAdDelegate` protocol as well as declare and connect instance variables to your UI .XIB:

```objective_c
#import <HADFramework/HADNativeAd.h>
@interface MyViewController : UIViewController <HADNativeAdDelegate>
	@property (nonatomic, weak) IBOutlet UIView *bannerView;
	@property (nonatomic, weak) IBOutlet UIImageView *imageView;
	@property (nonatomic, weak) IBOutlet UIImageView *iconView;
	@property (nonatomic, weak) IBOutlet UILabel *titleLabel;
	@property (nonatomic, weak) IBOutlet UILabel *descLabel;
	@property (nonatomic, strong) HADNativeAd *native;
@end
```

> Then, add a method in your View Controller's implementation file that initializes `HADNativeAd` and request an ad to load:

```objective_c
- (void)viewDidLoad {
	[super viewDidLoad];
	self.native = [[HADNativeAd alloc] initWithPlacementId:@"PLACEMENT_ID" bannerSize:kHADBannerSize300x250 delegate:self];
	self.bannerView.hidden = YES;
	[self.native loadAd];
}
```

> Now that you have added the code to load the ad, add the following functions to handle loading failures and to construct the ad once it has loaded:

```objective_c
- (void) HADNativeAd:(HADNativeAd*)nativeAd didFailWithError:(NSError*)error {
	NSLog(@"ERROR: %@",error.localizedDescription);
}

- (void) HADNativeAdDidLoad:(HADNativeAd*)nativeAd {
  self.imageView.image = nativeAd.image;
  self.iconView.image = nativeAd.icon;
  self.titleLabel.text = nativeAd.title;
  self.descLabel.text = nativeAd.desc;

	// Register the native ad view container for counting ad impressions
  [nativeAd registerAdView:self.bannerView];
	self.bannerView.hidden = NO;
}

- (void) HADNativeAdDidClick:(HADNativeAd*)nativeAd {
	NSLog(@"CLICKED AD");
}
```

> In cases where you re-use the view to show different ads over time, make sure to call `unregisterAdView` before registering the same view with a different instance of `HADNativeAd`, or end using ad view.

## Native Ads in Android

The Native Ad API allows you to build a customized experience for the ads you show in your app. When using the Native Ad API, instead of receiving an ad ready to be displayed, you will receive a group of ad properties such as a title, an image, a call to action, and you will have to use them to construct a custom view where the ad is shown.

**There are three actions you will need to take to implement this in your app:**

* Request an ad
* Use the returned ad metadata to build a custom native UI
* Register the ad's view with the `nativeAd` instance

### Set up the SDK

Follow these steps to download and include it in your project:

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidADs_Sample_v1.2.2.zip) and extract the Example app for Android.
* Open pdf file from archive for instructions.
* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidMoPubAdapter_1.1.0.zip) and extract the Mopub adapter if needed.
