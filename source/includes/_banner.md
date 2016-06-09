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

* Download and extract the HADFramework for iOS.
* Drag the HADFramework.framework file to your Xcode project and place it under the Frameworks folder.
* Add the AdSupport framework to your project.

### Implementation

> Add the following line to your AppDelegate implementation file:

```objective_c
#import <HADFramework/HADBannerView.h>
```

> Then, you can add:

```objective_c
- (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    [HADBannerView configureClass];

    return YES;
}
```

> Add the following line to your View Controller implementation file:

```objective_c
#import <HADFramework/HADBannerView.h>
```

> Then, on your View Controller's viewDidLoad implementation, use property of the HADBannerView class and add it to your view. Since HADBannerView is a subclass of UIView, you can add it to your view hierarchy just as with any other view:

```objective_c
- (void)viewDidLoad {
		[super viewDidLoad];

		self.bannerView = [[HADBannerView alloc] initWithSize:kHADBannerSizeHeight50 placementId:PLACEMENT_ID delegate:self];

		[self.bannerView setFrame:CGRectMake(0,0, self.view.frame.size.width, 50)];
		[self.view addSubview:self.bannerView];
		self.bannerView.hidden = YES;

		[self.bannerView loadAd];
}
```

> If you are building your app for iPad, consider using the `kHADBannerSizeHeight90` size instead. The HADFramework also supports the 300x250 ad size. Configure the HADBannerView with the following ad size: `kHADBannerSize300x250`:

```objective_c
- (void)viewDidLoad {
    [super viewDidLoad];

    self.bannerView = [[HADBannerView alloc] initWithSize:kHADBannerSize300x250 placementId:PLACEMENT_ID delegate:self];

    [self.bannerView setFrame:CGRectMake(10, 100, 300, 250)];
    [self.view addSubview:self.bannerView];
    self.bannerView.hidden = YES;

   	[self.bannerView loadAd];
}
```

> Now that you have the basic code running, it's recommended that you use the `HADBannerView` delegate to get notified when the ad fails to load so you could hide the banner unit. In the same way, you can add it back when it was loaded:
First, on your View Controller implementation file, declare that you are implementing the HADBannerViewDelegate protocol:

```objective_c
@interface ViewController : UIViewController <HADBannerViewDelegate>
 @property (strong, nonatomic) HADBannerView *bannerView;
@end
```

> Then, add and implement the following three functions in your View Controller implementation file to handle ad loading failures and completions:

```objective_c
- (void) HADViewDidLoad:(HADBannerView *)view {
    NSLog(@"AD LOADED");
    self.bannerView.hidden = NO;
}

- (void) HADView:(HADBannerView *)view didFailWithError:(NSError *)error {
    NSLog(@"ERROR: %@",error.localizedDescription);
}

- (void) HADViewDidClick:(HADBannerView *)view {
    NSLog(@"CLICKED AD");
}
```
