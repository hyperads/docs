# Banner

HyperAdX uses Placement ID to allow access to the API. You can register a new App and create Placement at our [developer portal](http://hyperadx.com/publishers/sign_in).

HyperAdX expects for Placement ID to be included in all API requests to the server in a get variable that looks like the following:

<aside class="notice">
You must replace <code>PLACEMENT_ID</code> with your placement's ID.
</aside>

## Mobile Web

* Go to the Publisher UI
* Create new App
* Create new Placement for it
* On placements list click on Tag & SDK and select appropriate integration.

## iOS

The HyperAdX Banner ads allows you to monetize your iOS apps with banner ads. This guide explains how to add banner ads to your app. If you're interested in other kinds of ad units, see the list of available types.

### Set up the SDK

**Manual**

* [Download](https://github.com/hyperads/ios-sdk/releases) and extract the HADFramework for iOS.
* Open your project target General tab.
* Drag the HADFramework.framework file to Embedded Binaries.
* Open your project target Build Settings tab. (Required only for Objective-C projects)
* Set "Embedded Content Contains Swift Code" to Yes. (Required only for Objective-C projects)
* Add the AdSupport framework to your project.

### iOS 7

If you want to support iOS7 - [download](https://github.com/hyperads/ios-sdk/releases/tag/v2.0.3) our legacy SDK. It supports only NativeAds.

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

> Ok, let's move to your View Controller. Import the SDK, declare that you implement the HADBannerViewDelegate protocol and add an instance variable for the interstitial ad unit:

```swift
import UIKit
import HADFramework

class MyViewController: UIViewController, HADBannerViewDelegate {
	var bannerView: HADBannerView!
}
```

> Then, on your View Controller's viewDidLoad implementation, use property of the HADBannerView class and add it to your view. Since HADBannerView is a subclass of UIView, you can add it to your view hierarchy just as with any other view:

```swift
override func viewDidLoad() {
	super.viewDidLoad()
	bannerView = HADBannerView(frame: CGRectMake(0,0, view.frame.size.width, 50))
	view.addSubview(bannerView);
	bannerView.loadAd("PLACEMENT_ID", bannerSize: .Height50, delegate: self)
}
```

> If you are building your app for iPad, consider using the `HADBannerSize.Height90` size instead. The HADFramework also supports the 300x250 ad size. Configure the HADBannerView with the following ad size: `HADBannerSize.Block300x250`:

```swift
override func viewDidLoad() {
    super.viewDidLoad()
    bannerView = HADBannerView(frame: CGRectMake(10, 100, 300, 250))
    view.addSubview(bannerView);
    bannerView.loadAd("PLACEMENT_ID", bannerSize: .Block300x250, delegate: self)
}
```

> Then, add and implement the following three functions in your View Controller implementation file to handle ad loading failures and completions:

```swift
//MARK: HADBannerViewDelegate

func HADViewDidLoad(view: HADBannerView) {
	print("AD LOADED")
}

func HADViewDidClick(view: HADBannerView) {
	print("CLICKED AD")
}

func HADView(view: HADBannerView, didFailWithError error: NSError?) {
	print("ERROR: %@", error?.localizedDescription)
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

> Ok, let's move to your View Controller. In implementation file, import the SDK header, declare that you implement the HADBannerViewDelegate protocol and add an instance variable for the banner view:

```objective_c
#import <HADFramework/HADFramework.h>

@interface MyViewController () <HADBannerViewDelegate>
@property (weak, nonatomic) IBOutlet HADBannerView *bannerView;
@end
```

> Then, on your View Controller's viewDidLoad implementation, use property of the HADBannerView class and add it to your view. Since HADBannerView is a subclass of UIView, you can add it to your view hierarchy just as with any other view:

```objective_c
-(void)viewDidLoad {
    [super viewDidLoad];
    self.bannerView = [[HADBannerView alloc] initWithFrame:CGRectMake(0,0, self.view.frame.size.width, 50)];
    [self.view addSubview:self.bannerView];
    [self.bannerView loadAd:@"PLACEMENT_ID" bannerSize:HADBannerSizeHeight50 delegate:self];
}
```

> If you are building your app for iPad, consider using the `HADBannerSizeHeight90` size instead. The HADFramework also supports the 300x250 ad size. Configure the HADBannerView with the following ad size: `HADBannerSizeBlock300x250`:

```objective_c
-(void)viewDidLoad {
    [super viewDidLoad];
    self.bannerView = [[HADBannerView alloc] initWithFrame:CGRectMake(10, 100, 300, 250)];
    [self.view addSubview:self.bannerView];
    [self.bannerView loadAd:@"PLACEMENT_ID" bannerSize:HADBannerSizeBlock300x250 delegate:self];
}
```

> Then, add and implement the following three functions in your View Controller implementation file to handle ad loading failures and completions:

```objective_c
#pragma mark - HADBannerViewDelegate

-(void)HADViewDidLoad:(HADBannerView *)view {
    NSLog(@"HADViewDidLoad");
}

-(void)HADView:(HADBannerView *)view didFailWithError:(NSError *)error {
    NSLog(@"HADViewDidFai:l %@", error);
}

-(void)HADViewDidClick:(HADBannerView *)view {
    NSLog(@"HADViewDidClick");
}
```
