# Barcode Scanning Module for React Native

A barcode scanner component for React Native built on top of [Dynamsoft Mobile Barcode SDK](https://www.dynamsoft.com/barcode-reader/sdk-mobile/). 

## What You Should Know
- [![](https://img.shields.io/badge/Download-Offline%20SDK-orange)](https://www.dynamsoft.com/barcode-reader/downloads)
- [![](https://img.shields.io/badge/Get-30--day%20FREE%20Trial%20License-blue)](https://www.dynamsoft.com/customer/license/trialLicense/?product=dbr)

## Try Barcode Scanner Demo App

[<kbd><img src="https://www.dynamsoft.com/webres/wwwroot/images/icons/Google-play.svg" width="50%">](https://play.google.com/store/apps/details?id=com.dynamsoft.demo.dynamsoftbarcodereaderdemo)
    
![Barcode Scanner X](https://www.dynamsoft.com/webres/wwwroot/images/icons/google-play-qrcode.png)

[<kbd><img src="https://www.dynamsoft.com/webres/wwwroot/images/icons/apple-store.svg" width="50%">](https://itunes.apple.com/us/app/barcode-scanner-x/id1120581630?mt=8)
    
![Barcode Scanner X](https://www.dynamsoft.com/webres/wwwroot/images/icons/app-store-qrcode.png)

## Contact Us
If there are any questions, please feel free to contact support@dynamsoft.com.


## Development Requirements

- Node
- JDK
- Xcode 
- Android Studio. 

## How to Run the Example

### iOS
Run `npm install or yarn (npm install -g yarn)` from the Example directory first, then `pod install` in the react-native-dbr/Libraries.
And make sure `Your Project Target -> Build Settings -> Search Paths -> Frameworks Search Paths` and `Pods-BarcodeReaderManager.debug(release).xcconfig`:
```bash
Frameworks Search Paths = "${PODS_ROOT}/DynamsoftBarcodeReader"
HEADER_SEARCH_PATHS = $(inherited) "${PODS_ROOT}/DynamsoftBarcodeReader/DynamsoftBarcodeReader.framework/Headers"
OTHER_LDFLAGS = $(inherited) -ObjC -framework "DynamsoftBarcodeReader"
```
Then `react-native run-ios`.

### Android
```bash
npm install or yarn (npm install -g yarn)
react-native run-android
```

### Screenshots

<kbd><img src="http://www.codepool.biz/wp-content/uploads/2017/04/react-native-barcode-detection.jpg" width="50%">

<kbd><img src="http://www.codepool.biz/wp-content/uploads/2017/04/react-native-barcode-result.png" width="50%">

## How to Use the Barcode Scanning Module

### In Android

1. Create a new React Native project if you don't have one.

    ```bash
    react-native init NewProject
    ```

2. Add the local module to dependencies in `package.json`.

    ```json
    "dependencies": {
        "react": "16.9.0",
        "react-native": "^0.61.1",
        "react-native-dbr": "^8.0.1"
    },
    ```

3. On `android/settings.gradle`.

    ```
    include ':react-native-dbr'
    project(':react-native-dbr').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-dbr/androidLib')
   ```

4. Add `':react-native-dbr'` dependency in `android/app/build.gradle`.

    ```
    dependencies {
        implementation project(path: ':react-native-dbr')
    }
    ```
5. On the MainApplication of your Android project add the import of BarcodeReaderPackage line to:

    ```java
    import com.demo.barcodescanner.BarcodeReaderPackage;
    
    ...
    @Override
    protected List<ReactPackage> getPackages() {
      @SuppressWarnings("UnnecessaryLocalVariable")
      List<ReactPackage> packages = new PackageList(this).getPackages();
      packages.add(new BarcodeReaderPackage()); // add BarcodeReaderPackage here!!
      return packages;
    }
    ...
    
    ```
6. Use the module in `App.js`.

    ```javascript
    import {NativeModules} from 'react-native';
    const BarcodeReaderManager = NativeModules.BarcodeReaderManager;
    
    //android
    BarcodeReaderManager.readBarcode('your license here',events => {
      this.setState({result: events});
    },err => {
      alert(err);
    }
    );
    ```

### In iOS

1. Create a new React Native project if you don't have one.

    ```bash
    react-native init NewProject
    ```

2. Add the local module to dependencies in `NewProject/package.json`.

    ```json
    "dependencies": {
        "react": "16.9.0",
        "react-native": "^0.61.1",
        "react-native-dbr": "^8.0.1"
    }
    ```

3. Remove `node_moudules` and install.

    ```bash
    sudo rm -rf node_moudules 
    npm install or yarn
    ```

4. Add `BarcodeReaderManager.xcodeproj` to  your project libraries. And make sure `Pods/Target Support Files/Pods-BarcodeReaderManager.debug.xcconfig`:

```
  FRAMEWORK_SEARCH_PATHS = "${PODS_ROOT}/DynamsoftBarcodeReader"
  HEADER_SEARCH_PATHS = "${PODS_ROOT}/DynamsoftBarcodeReader/DynamsoftBarcodeReader.framework/Headers"
  LIBRARY_SEARCH_PATHS = "${PODS_ROOT}/DynamsoftBarcodeReader"
  OTHER_LDFLAGS = -framework "DynamsoftBarcodeReader"
```

5.  In your NewProject: 

```
  Project -> Build Settings -> FRAMEWORK_SEARCH_PATHS = `$(PROJECT_DIR)/../Libraries` 
  HEADER_SEARCH_PATHS = `$(PROJECT_DIR)/../Libraries`
```

6. Modify the module in `App.js`(different from android).

    ```javascript
    import {NativeModules} from 'react-native';
    const BarcodeReaderManager = NativeModules.BarcodeReaderManager;
    
    //ios
    BarcodeReaderManager.readBarcode('your license here').then((msg) =>{
        this.setState({result: msg});
    }).catch((err) => {
        console.log(err);
    });
    ```

7. To achieve navigation from react-native to viewController, in `AppDelegate.h` and `AppDelegate.m`, add the following code:

    ```AppDelegate.h:
    ...
    @property (nonatomic, strong) UIWindow *window;
    @property (nonatomic, strong) UINavigationController *nav;
    @property (nonatomic, strong) RCTRootView *rootView;
    @property (nonatomic, strong) UIViewController *rootViewController;
    ...
    
    ```AppDelegate.m:
    #import "../../../ios/BarcodeReaderManagerViewController.h"
    #import "../../../ios/DbrManager.h"

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
    //  self.window.rootViewController = rootViewController;
        _nav = [[UINavigationController alloc] initWithRootViewController:_rootViewController];
        self.window.rootViewController = _nav;
        _nav.navigationBarHidden = YES;
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(doNotification:) name:@"readBarcode" object:nil];
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(backToJs:) name:@"backToJs" object:nil];
        [self.window makeKeyAndVisible];
        return YES;
    }

    -(void)doNotification:(NSNotification *)notification{
        BarcodeReaderManagerViewController* dbrMangerController = [[BarcodeReaderManagerViewController alloc] init];
        dbrMangerController.dbrManager = [[DbrManager alloc] initWithLicense:notification.userInfo[@"inputValue"]];
        [self.nav pushViewController:dbrMangerController animated:YES];
    }

    -(void)backToJs:(NSNotification *)notification{
        [self.nav popToViewController:self.rootViewController animated:YES];
    }
    ```

