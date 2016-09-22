# Native ads

HyperAdX uses Placement ID to allow access to the API. You can register a new App and create Placement at our [developer portal](http://hyperadx.com/publishers/sign_in).

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

* [Download](https://github.com/hyperads/ios-sdk/releases) latest release and extract the HADFramework for iOS.
* Open your project target General tab.
* Drag the HADFramework.framework file to Embedded Binaries.
* Open your project target Build Settings tab. (Required only for Objective-C projects)
* Set "Embedded Content Contains Swift Code" to Yes. (Required only for Objective-C projects)
* Add the AdSupport framework to your project.

### iOS 7

If you want to support iOS7 - [download](https://github.com/hyperads/ios-sdk/releases/tag/v2.0.3) our legacy SDK. It supports only NativeAds.

### Swift implementation

> Now, in your View Controller implementation file, import the SDK and declare that you implement the `HADNativeAdDelegate` protocol as well as declare and connect instance variables to your Storyboard or .XIB:

```swift
import HADFramework
class MyViewController: UIViewController, HADNativeAdDelegate {
    @IBOutlet weak var bannerView: UIView!
    @IBOutlet weak var imageView: HADMediaView!
    @IBOutlet weak var iconView: HADMediaView!
    @IBOutlet weak var titleLabel: UILabel!
    @IBOutlet weak var descLabel: UILabel!
    @IBOutlet weak var cta: UIButton?

    var nativeAd: HADNativeAd! = nil

}
```

> Then, add a method in your View Controller's implementation file that initializes `HADNativeAd` and request an ad to load:

```swift
override func viewDidLoad() {
	super.viewDidLoad()
	nativeAd = HADNativeAd(placementId: "PLACEMENT_ID", content: [.Title, .Description, .Banner, .Icon], delegate: self)
	nativeAd.loadAd()
}
```

>You may set `content` param on HADNativeAd initialization to get only needed properties. If you didn't set `content` param then you get all properties.

```
```

> to get title text

```swift
.Title
```

> to get description text

```swift
.Description
```

> to get banner

```swift
.Banner
```

> to get icon

```swift
.Icon
```

> Now that you have added the code to load the ad, add the following functions to handle loading failures and to construct the ad once it has loaded:

```swift
//MARK: HADNativeAd Delegate
func HADAd(nativeAd: HADNativeAd, didFailWithError error: NSError) {
	print("ERROR: \(error.localizedDescription)")
}

func HADNativeAdDidLoad(nativeAd: HADNativeAd) {
	imageView.loadHADBanner(nativeAd, animated: true) { (error, image) in
    		if error != nil {
        		print("ERROR: CAN'T DOWNLOAD BANNER \(error)")
        		return
    		}
    		print("BANNER DOWNLOADED")
	}
	iconView.loadHADIcon(nativeAd, animated: true) { (error, image) in
    		if error != nil {
        		print("ERROR: CAN'T DOWNLOAD ICON \(error)")
        		return
    		}
    		print("ICON DOWNLOADED")
	}
	titleLabel.text = nativeAd.title
	descLabel.text = nativeAd.desc
	cta?.setTitle(nativeAd.cta, forState: .Normal)
}

func HADNativeAdDidClick(nativeAd: HADNativeAd) {
	print("CLICKED AD")
}
```

> Handle click on your implementation of "call to action" button

```swift
@IBAction func adClicked() {
	nativeAd.handleClick()
}
```

### Objective-C implementation

> Now, in your View Controller implementation file, import the SDK header and declare that you implement the `HADNativeAdDelegate` protocol as well as declare and connect instance variables to your Storyboard or .XIB:

```objective_c
#import <HADFramework/HADFramework.h>
@interface MyViewController : UIViewController <HADNativeAdDelegate>
	@property (strong, nonatomic) HADNativeAd *nativeAd;
	@property (weak, nonatomic) IBOutlet UIView *bannerView;
	@property (weak, nonatomic) IBOutlet HADMediaView *bannerMediaView;
	@property (weak, nonatomic) IBOutlet HADMediaView *iconMediaView;
	@property (weak, nonatomic) IBOutlet UILabel *titleLabel;
	@property (weak, nonatomic) IBOutlet UILabel *descriptionLabel;
	@property (weak, nonatomic) IBOutlet UIButton *ctaButton;
@end
```

> Then, add a method in your View Controller's implementation file that initializes `HADNativeAd` and request an ad to load:

```objective_c
- (void)viewDidLoad {
	[super viewDidLoad];
	self.nativeAd = [[HADNativeAd alloc] initWithPlacementId:@"PLACEMENT_ID" content:@[HADAdContentTitle, HADAdContentDescription, HADAdContentBanner, HADAdContentIcon] delegate:self];
    	[self.nativeAd loadAd];
}
```

> You may set `content` param on HADNativeAd initialization to get only needed properties. If you didn't set `content` param then you get all properties.

```
```

> to get title text

```objective_c
HADAdContentTitle
```

> to get description text

```objective_c
HADAdContentDescription
```

> to get banner

```objective_c
HADAdContentBanner
```

> to get icon

```objective_c
HADAdContentIcon
```

> Now that you have added the code to load the ad, add the following functions to handle loading failures and to construct the ad once it has loaded:

```objective_c
-(void)HADNativeAdDidFail:(HADNativeAd *)nativeAd error:(NSError *)error
	NSLog(@"ERROR: %@",error.localizedDescription);
}

-(void)HADNativeAdDidLoad:(HADNativeAd *)nativeAd {
	[self.bannerMediaView loadHADBanner:nativeAd animated:NO completion:^(NSError * _Nullable error, UIImage * _Nullable image) {
		if (!error) {
    			NSLog(@"Banner downloaded");
		} else {
    			NSLog(@"Banner download error: %@", error);
		}
	}];
	[self.iconMediaView loadHADIcon:nativeAd animated:NO completion:^(NSError * _Nullable error, UIImage * _Nullable image) {
		if (!error) {
    			NSLog(@"Icon downloaded");
		} else {
    			NSLog(@"Icon download error: %@", error);
		}
	}];
	[self.titleLabel setText:nativeAd.title];
	[self.descriptionLabel setText:nativeAd.desc];
	[self.ctaButton setTitle:nativeAd.cta forState:UIControlStateNormal];
}

-(void)HADNativeAdDidClick:(HADNativeAd *)nativeAd
	NSLog(@"CLICKED AD");
}
```

> Handle click on your implementation of "call to action" button

```objective_c
- (IBAction)handleClick:(id)sender {
	[self.nativeAd handleClick];
}
```

## Native Ad iOS templates

The Native Ad templates allows you to use prepared Ad banner views but with possibility of full customization.

Just add HADBannerTemplateView to your view controller and set desired banner template and custom params.

### You can choose from six layouts:

Layout | Description
--------- | -----------
`HADBannerTemplateTypes.BlockOne` | Flexible block banner with aspect ratio 320:230
`HADBannerTemplateTypes.BlockTwo` | Flexible block banner with aspect ratio 320:300
`HADBannerTemplateTypes.BlockThree` | Flexible block banner with aspect ratio 320:340
`HADBannerTemplateTypes.LineOne` | Line banner with 50pt height
`HADBannerTemplateTypes.LineTwo` | Line banner with 60pt height
`HADBannerTemplateTypes.LineThree` | Line banner with 90pt height

### Custom params

All custom params starts with `custom` prefix

Group | Param | Description
--------- | ----------- | -----------
Background color | `customBackgroundColor` | Whole ad background
Background color | `customButtonBackgroundColor` | CTA button background
Background color | `customAgeRatingBackgroundColor` | AgeRating label background
Text colors | `customTitleTextColor` | Title label text color
Text colors | `customDescriptionTextColor` | Description label text color
Text colors | `customPoweredByTextColor` | PoweredBy label text color
Text colors | `customAgeRatingTextColor` | AgeRating label text color
Text colors | `customButtonTitleColor` | CTA button title text color
Corner radius | `customIconCornerRadius` | Icon
Corner radius | `customBannerCornerRadius` | Banner
Corner radius | `customButtonCornerRadius` | CTA button
Corner radius | `customAgeRatingCornerRadius` | Age rating label
CTA button border | `customButtonBorderColor` | Border color
CTA button border | `customButtonBorderWidth` | Border width
Star rating | `customStarRatingFilledColor` | Filled color
Star rating | `customStarRatingEmptyColor` | Empty color
Star rating | `customStarRatingTextColor` | Right text color
Click mode | `customClickMode = .Button` | Handle click only on button
Click mode | `customClickMode = .WholeBanner` | Handle click everywhere

### Swift example

```swift
import HADFramework

class MyViewController: UIViewController, HADBannerTemplateViewDelegate {
    @IBOutlet weak var bannerTemplateView: HADBannerTemplateView!

    override func viewDidLoad() {
        super.viewDidLoad()
        //Just set HADBannerTemplateTypes param in loadAd method
        bannerTemplateView.loadAd("PLACEMENT_ID", bannerTemplate: .One, delegate: self)
        //And customize everything
        bannerTemplateView.customBackgroundColor = UIColor.lightGrayColor()
        bannerTemplateView.customTitleTextColor = UIColor.blueColor()
        bannerTemplateView.customDescriptionTextColor = UIColor.darkGrayColor()
        bannerTemplateView.customIconCornerRadius = 6
        bannerTemplateView.customButtonBackgroundColor = UIColor.clearColor()
        bannerTemplateView.customButtonBorderColor = UIColor.purpleColor()
        bannerTemplateView.customButtonBorderWidth = 1
        bannerTemplateView.customButtonTitleColor = UIColor.purpleColor()
        bannerTemplateView.customButtonCornerRadius = 6
        bannerTemplateView.customBannerCornerRadius = 6
        bannerTemplateView.customStarRatingFilledColor = UIColor.greenColor()
        bannerTemplateView.customStarRatingEmptyColor = UIColor.purpleColor()
        bannerTemplateView.customStarRatingTextColor = UIColor.purpleColor()
        bannerTemplateView.customClickMode = .Button
    }

    //MARK: HADBannerTemplateViewDelegate

    func HADTemplateViewDidLoad(view: HADBannerTemplateView) {
        print("AD LOADED")
    }

    func HADTemplateViewDidClick(view: HADBannerTemplateView) {
        print("CLICKED AD")
    }

    func HADTemplateView(view: HADBannerTemplateView, didFailWithError error: NSError?) {
        print("ERROR: %@", error?.localizedDescription)
    }
}
```

### Objective-C example

```objective_c
#import <HADFramework/HADFramework.h>

@interface MyViewController () <HADBannerTemplateViewDelegate>
@property (weak, nonatomic) IBOutlet HADBannerTemplateView *bannerTemplateView;
@end

@interface MyViewController : UIViewController <HADNativeAdDelegate>
-(void)viewDidLoad {
	[super viewDidLoad];
	//Just set HADBannerTemplateTypes param in loadAd method
	[self.bannerTemplateView loadAd:@"PLACEMENT_ID" bannerTemplate:HADBannerTemplateTypesBlockOne delegate:self];
	//And customize everything
	[self.bannerTemplateView setCustomBackgroundColor:[UIColor lightGrayColor]];
	[self.bannerTemplateView setCustomTitleTextColor:[UIColor blackColor]];
	[self.bannerTemplateView setCustomDescriptionTextColor:[UIColor darkGrayColor]];
	[self.bannerTemplateView setCustomIconCornerRadius:6.0];
	[self.bannerTemplateView setCustomButtonBackgroundColor:[UIColor clearColor]];
	[self.bannerTemplateView setCustomButtonBorderColor:[UIColor purpleColor]];
	[self.bannerTemplateView setCustomButtonBorderWidth:1.0];
	[self.bannerTemplateView setCustomButtonTitleColor:[UIColor purpleColor]];
	[self.bannerTemplateView setCustomButtonCornerRadius:6.0];
	[self.bannerTemplateView setCustomBannerCornerRadius :6.0];
	[self.bannerTemplateView setCustomAgeRatingCornerRadius:4.0];
	[self.bannerTemplateView setCustomStarRatingFilledColor:[UIColor greenColor]];
	[self.bannerTemplateView setCustomStarRatingEmptyColor:[UIColor purpleColor]];
	[self.bannerTemplateView setCustomStarRatingTextColor:[UIColor purpleColor]];
	[self.bannerTemplateView setCustomClickMode:BannerTemplateCustomClickModesButton];
}

#pragma mark - HADBannerTemplateViewDelegate

-(void)HADTemplateViewDidLoad:(HADBannerView *)view {
	NSLog(@"HADTemplateViewDidLoad");
}

-(void)HADTemplateView:(HADBannerView *)view didFailWithError:(NSError *)error {
	NSLog(@"HADTemplateViewDidFai:l %@", error);
}

-(void)HADTemplateViewDidClick:(HADBannerView *)view {
	NSLog(@"HADTemplateViewDidClick");
}

@end
```

## Native Ads in Android

The Native Ad API allows you to build a customized experience for the ads you show in your app. When using the Native Ad API, instead of receiving an ad ready to be displayed, you will receive a group of ad properties such as a title, an image, a call to action, and you will have to use them to construct a custom view where the ad is shown.

Sample projects:

* [Download](https://github.com/hyperads/android-sdk/releases) latest release and extract the Example app for Android.

### Set up the SDK

>  Add following under manifest tag to your AndroidManifest.xml:

```xml
 <uses-permission android:name="android.permission.INTERNET"/>
 <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
```

>  Put the HyperAdxSDK_xxx.jar in “libs” folder in your Android Studio or Eclipse. Add it to dependencies in build.grandle file. Also you need to add google play services.

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
>  The next step is to extract the ad metadata and use its properties to build your customized native UI. You can either create your custom view in a layout .xml, or you can add elements in code. The custom layout .xml. For example:

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

>  Modify the onAdLoaded function above to retrieve the ad properties. The SDK will log the impression and handle the click automatically.

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
> If you want to use Native AD in RecyclerView you should use registerViewForInteraction(Ad ad, View adView) method instead of getNativeAdView(Ad ad, int ResID)
Sample: 

```java

   @Override
    public void onBindViewHolder(final MyViewHolder holder, int position) {

        if (getItemViewType(position) == AD_TYPE) {

            final HADNativeAd nativeAd = new HADNativeAd(activity,
                    activity.getString(R.string.Placement)
            ); //Native AD constructor

            nativeAd.setContent("title,icon,description"); // Set content to load
            nativeAd.setAdListener(new AdListener() { // Add Listeners

                @Override
                public void onAdLoaded(Ad ad) { // Called when AD is Loaded
                    Toast.makeText(activity, "Native ad loaded", Toast.LENGTH_SHORT).show();

                    holder.ivIcon.setVisibility(View.VISIBLE);


                    nativeAd.registerViewForInteraction(ad, holder.rlRoot); // Configuring your view

                    //  Setting the Text.
                    holder.title.setText(ad.getTitle());
                    holder.genre.setText(ad.getDescription());
                    //  Downloading and setting the ad icon.
                    HADNativeAd.downloadAndDisplayImage(holder.ivIcon, ad.getIcon_url());

                }

                @Override
                public void onError(Ad nativeAd, String error) { // Called when load is fail
                    Toast.makeText(activity, "Native Ad failed to load with error: " + error, Toast.LENGTH_SHORT).show();

                }

                @Override
                public void onAdClicked() { // Called when user click on AD
                    Toast.makeText(activity, "Tracked Native Ad click", Toast.LENGTH_SHORT).show();

                }
            });

            nativeAd.loadAd(); // Call to load AD


        } else {

            holder.ivIcon.setVisibility(View.GONE);

            Movie movie = moviesList.get(position);
            holder.title.setText(movie.getTitle());
            holder.genre.setText(movie.getGenre());
            holder.year.setText(movie.getYear());
        }
    }

```
