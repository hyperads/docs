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

Sample projects:

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidADs_Sample_v1.2.3.zip) and extract the Example app for Android.
* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidMoPubAdapter_1.1.0.zip) and extract the Mopub adapter if needed.

### Set up the SDK

>  Add following under manifest tag to your AndroidManifest.xml:
```xml
 <uses-permission android:name="android.permission.INTERNET"/>
```
>  Put the HyperAdxSDK_xxx.jar in “libs” folder in your Android Studio or Eclipse

>  Add it to dependencies in build.grandle file . Also you need to add google play services.

```groove
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    compile 'com.android.support:appcompat-v7:23.4.0'
    compile 'com.android.support:support-v4:23.4.0'
    compile 'com.google.android.gms:play-services-ads:9.0.2'
    compile 'com.google.android.gms:play-services-base:9.0.2'
}
```

>  Then, create a function that requests a native ad:

```java
private void showNativeAd() {
    adFrame = (FrameLayout) findViewById(R.id.adContent);
    nativeAd = new HADNativeAd(this, "YOUR_PLACEMENT_ID"); //Native AD constructor
    nativeAd.setContent("title,icon,main,description"); // Set content to load
    nativeAd.setAdListener(new AdListener() { // Add Listeners
        @Override
        public void onAdLoaded(Ad ad) { // Called when AD is Loaded
           
        }
        @Override
        public void onError(Ad nativeAd, String error) { // Called when load is fail
            
        }

        @Override
        public void onAdClicked() { // Called when user click on AD
            
        }
    });
    nativeAd.loadAd(); // Call to load AD
}
```
>  The next step is to extract the ad metadata and use its properties to build your customized native UI. You can either create your custom view in a layout .xml, or you can add elements in code.

> The custom layout .xml. For example:
 
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
    <ImageView
        android:id="@+id/ivIcon"
        android:layout_width="50dp"
        android:layout_height="50dp"
        android:layout_alignParentLeft="true"
        android:layout_alignParentTop="true" />
    <TextView
        android:id="@+id/tvTitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentTop="true"
        android:layout_toRightOf="@+id/ivIcon"
        android:paddingLeft="10dp"
        android:textSize="18sp"
        android:textStyle="bold"
        android:typeface="monospace" />
    <ImageView
        android:id="@+id/ivImage"
        android:layout_width="480dp"
        android:layout_height="168dp"
        android:layout_alignParentLeft="true"
        android:layout_below="@+id/ivIcon" />
    <TextView
        android:id="@+id/tvDescription"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_below="@+id/ivImage" />
</RelativeLayout>
```

> Now you can use this  layout .xml as a frame. For example:
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!" />
    <FrameLayout
        android:id="@+id/adContent"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_centerHorizontal="true" >
    </FrameLayout>
</RelativeLayout>
```
>  Modify the onAdLoaded function above to retrieve the ad properties. For example: 

```java
private NativeAd nativeAd;
private View AdView;
private FrameLayout adFrame; //FrameLayout with all views that you need
-------------------------------------------------------------------------
@Override
public void onAdLoaded(Ad ad) { // Called when AD is Loaded
    Toast.makeText(MainActivity.this, "Native ad loaded", Toast.LENGTH_SHORT).show();
    AdView = nativeAd.getNativeAdView(ad, R.layout.native_ad_layout); // Registering view for AD
    adFrame.addView(AdView); //Adding view to frame
    // Create native UI using the ad metadata.
    TextView tvTitle = (TextView) AdView.findViewById(R.id.tvTitle);
    TextView tvDescription = (TextView) AdView.findViewById(R.id.tvDescription);
    ImageView ivIcon = (ImageView) AdView.findViewById(R.id.ivIcon);
    ImageView ivImage = (ImageView) AdView.findViewById(R.id.ivImage);
    // Setting the Text.
    tvTitle.setText(ad.getTitle());
    tvDescription.setText(ad.getDescription());
    // Downloading and setting the ad icon.
    NativeAd.downloadAndDisplayImage(ivIcon, ad.getIcon_url());
    // Download and setting the cover image.
    NativeAd.downloadAndDisplayImage(ivImage, ad.getImage_url());
}
```
> The SDK will log the impression and handle the click automatically.
