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

Sample projects:

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidADs_Sample_v1.2.3.zip) and extract the Example app for Android.

### Set up the SDK

> Add following under manifest tag to your AndroidManifest.xml:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
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

>  First of all you need to add new app in AdMob console.
> You will get UnitId string like 'ca-app-pub-*************/*************'.

> For the next few hours you may get the AdMob errors with codes 0 or 2. Just be patient.

>  Then you need to add new mediation source.
> Fill 'Class Name ' field with a 'com.hyperadx.admob.HADInterstitialEvent' string. And a 'Parameter' with your HyperAdx statement string.

> Setup eCPM for new network

> Now you can setting up your android project.
> Put HyperAdx-SDK and AdMob-adapter in 'libs' folder.

> Add those lines in your build.gradle file:

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
**NOTE** - Admob interstitial may not work in emulator. Use real devices even for tests!

###Mopub Adapter

* [Download](https://s3-us-west-2.amazonaws.com/adpanel-public/HyperadxAndroidMoPubAdapter_1.1.0.zip) and extract the Mopub adapter if needed.

> You can use Hyperadx as a Network in Mopub's Mediation platform.
>  Setup SDKs:
>  - Integrate with Mopub SDK (https://github.com/mopub/mopub-android-sdk/wiki/Getting-Started)
>  - Install Hyperadx SDK 

> Setup Mopub Dashboard
> Create an "Hyperadx" Network in Mopub's dashboard and connect it to your Ad Units.
> In Mopub's dashboard select Networks > Add New network

> Then select Custom Native Network

> Complete the fields accordingly to the Ad Unit that you want to use


```java
Custom Event Class :
com.mopub.mobileads.HyperadxInterstitialMopub
```

```json
Custom Event Class Data :
{"PLACEMENT":"<YOUR PLACEMENT>"}

You can use the test placement "5b3QbMRQ"
```

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
