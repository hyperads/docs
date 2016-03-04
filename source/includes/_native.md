# Native ads

Dispply uses Access tokens to allow access to the API. You can register a new App and get Access token key at our [developer portal](http://dispply.com/publishers/sign_in).

Dispply expects for the Access token and Placement ID to be included in all API requests to the server in a get variable that looks like the following:

<aside class="notice">
You must replace <code>TOKEN</code> with your app's Access token and <code>PLACEMENT_ID</code> with your placement's ID.
</aside>

## Native Ads in Mobile Web

To add ad to your mobile site copy and paste this code to web page

```html
<script async type="text/javascript">
(function(){
	var _s=document.createElement('script');
	_s.type='text/javascript';
	_s.async=true;
	_s.src='http://ad-cdn.dispply.com/v2/sdk.js';
	(document.body)?document.body.appendChild(_s):document.head.appendChild(_s);
	})();
</script>
<div class="w_NVM9zaqJ" ad-token="TOKEN" ad-placement="PLACEMENT_ID" ad-theme="base"></div>
```

> Also you can use custom template using [dot.js](http://olado.github.io/doT/index.html) template engine

```html
<script async type="text/javascript">
var w_NVM9zaqJ = {
  template: '
    <div class="dispply_board dispply_board-single">\
      <div class="centrifier">\
        <h1>App {{= it.status}}</h1>\
        {{~it.ads :ad:index}}\
          <article class="app-widget app-brick">\
            <a class="app-widget_button button-green" href="{{=ad.click_url}}" target="_blank">FREE</a>\
            <img src="{{=ad.creatives.icon.url}}" width="64" height="64" class="app-widget_ico">\
            <div class="app-widget_content">\
              <div class="app-widget_desc">\
                <h2>{{=ad.title}}</h2>\
                <p>{{=ad.description}}</p>\
              </div>\
            </div>\
            <a href="{{=ad.click_url}}" target="_blank" class="cover-link"></a>\
          </article>\
          {{~ad.beacons :url:index}}\
            <img style="position: absolute;visibility: hidden;" src="{{= url}}">\
          {{~}}\
        {{~}}\
      </div>\
    </div>'
}

(function(){
	var _s=document.createElement('script');
	_s.type='text/javascript';
	_s.async=true;
	_s.src='http://ad-cdn.dispply.com/v2/sdk.js';
	(document.body)?document.body.appendChild(_s):document.head.appendChild(_s);
	})();
</script>
<div class="w_NVM9zaqJ" ad-token="TOKEN" ad-placement="PLACEMENT_ID"></div>
```

## Native Ads in iOS

The Dispply's Native Ads allows you to build a customized experience for the ads you show in your app. When using the Native Ad API, instead of receiving an ad ready to be displayed, you will receive a group of ad properties such as a title, an image, a call to action, and you will have to use them to construct a custom UIView where the ad is shown.

**There are three actions you will need to take to implement this in your app:**

* Request an ad
* Use the returned ad metadata to build a custom native UI
* Register the ad's view with the `nativeAd` instance

### Set up the SDK

Follow these steps to download and include it in your project:

**Using Cocoapods**

* Add the following line to your project's Podfile: `pod 'DispplyNetwork'`.
* Run pod install.

**Manual**

* Download and extract the Dispply SDK for iOS.
* Drag the Dispply.framework file to your Xcode project and place it under the Frameworks folder.

### Implementation

> Now, in your View Controller header file, import the SDK header and declare that you implement the DispplyNativeAdDelegate protocol as well as declare and connect instance variables to your UI .XIB:

```objective_c
#import <UIKit/UIKit.h>
@import DispplyNetwork; // import Dispply module

@interface MyViewController : UIViewController <DispplyNativeAdDelegate>
  // Other code might go here...
  @property (weak, nonatomic) IBOutlet UIImageView *adIconImageView;
  @property (weak, nonatomic) IBOutlet UILabel *adTitleLabel;
  @property (weak, nonatomic) IBOutlet UILabel *adBodyLabel;
  @property (weak, nonatomic) IBOutlet UIButton *adCallToActionButton;
  @property (weak, nonatomic) IBOutlet UILabel *adSocialContextLabel;
  @property (weak, nonatomic) IBOutlet UILabel *sponsoredLabel;

  @property (weak, nonatomic) DispplyMediaView *adCoverMediaView;

  @property (weak, nonatomic) IBOutlet UIView *adView;
@end
```

## Native Ads in Android

The Native Ad API allows you to build a customized experience for the ads you show in your app. When using the Native Ad API, instead of receiving an ad ready to be displayed, you will receive a group of ad properties such as a title, an image, a call to action, and you will have to use them to construct a custom view where the ad is shown.

<aside class="notice">
There are three actions you will need to take to implement this in your app:
</aside>

* Request an ad
* Use the returned ad metadata to build a custom native UI
* Register the ad's view with the `nativeAd` instance

```java
private NativeAd nativeAd;

private void showNativeAd() {
  nativeAd = new NativeAd(this, "YOUR_PLACEMENT_ID");
  nativeAd.setAdListener(new AdListener() {
    @Override
    public void onError(Ad ad, AdError error) {

    }

    @Override
    public void onAdLoaded(Ad ad) {

    }

    @Override
    public void onAdClicked(Ad ad) {

    }
  });

  nativeAd.loadAd();
}
```
