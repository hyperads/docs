# Banner

Dispply uses Access tokens to allow access to the API. You can register a new App and get Access token key at our [developer portal](http://dispply.com/publishers/sign_in).

Dispply expects for the Access token and Placement ID to be included in all API requests to the server in a get variable that looks like the following:

<aside class="notice">
You must replace <code>TOKEN</code> with your app's Access token and <code>PLACEMENT_ID</code> with your placement's ID.
</aside>

## Mobile Web

To add ad to your mobile site copy and paste this code to web page

```html
<script async type="text/javascript">
(function(){
	var _s=document.createElement('script');
	_s.type='text/javascript';
	_s.async=true;
	_s.src='http://ad-cdn.dispply.com/v2/banner.js';
	(document.body)?document.body.appendChild(_s):document.head.appendChild(_s);
	})();
</script>
<div class="w_NVM9zaqJ" ad-token="TOKEN" ad-placement="PLACEMENT_ID" ad-format="120x90" style="text-align: center"></div>
```

## iOS

The Dispply Banner ads allows you to monetize your iOS apps with banner ads. This guide explains how to add banner ads to your app. If you're interested in other kinds of ad units, see the list of available types.

### Set up the SDK

Follow these steps to download and include it in your project:

**Using Cocoapods**

* Add the following line to your project's Podfile: pod 'DispplyNetwork'
* Run `pod install`.

**Manual**
* Download and extract the Dispply SDK for iOS.
* Drag the DispplyNetwork.framework file to your Xcode project and place it under the Frameworks folder.

### Implementation

> Now you'll need to import the Audience Network SDK header. Add the following line to your View Controller header file:

```objective_c
@import DispplyNetwork;
```

> Then, on your View Controller's viewDidLoad implementation, create a new instance of the DispplyAdView class and add it to your view. Since DispplyAdView is a subclass of UIView, you can add it to your view hierarchy just as with any other view.

```objective_c
- (void)viewDidLoad
{
  [super viewDidLoad];

  DispplyAdView *adView = [[DispplyAdView alloc] initWithPlacementID:PLACEMENT_ID
                         adSize:kDispplyAdSizeHeight50Banner
                         rootViewController:self];
  [adView loadAd];
  [self.view addSubview:adView];
}
```

## Android
