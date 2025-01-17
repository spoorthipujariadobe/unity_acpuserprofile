# Adobe Experience Platform - UserProfile plugin for Unity apps

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
    - [Initialization](#initialization)
    - [UserProfile methods](#UserProfile-methods)
- [Running Tests](#running-tests)
- [Sample App](#sample-app)
- [Contributing](#contributing)
- [Licensing](#licensing)

## Prerequisites

The `Unity Hub` application is required for development and testing. Inside of `Unity Hub`, you will be required to download the `Unity` app. The ACPUserProfile Unity package is built using Unity version 2019.4.

[Download the Unity Hub](http://unity3d.com/unity/download). The free version works for development and testing, but a Unity Pro license is required for distribution. See [Distribution](#distribution) below for details.

#### FOLDER STRUCTURE
Plugins for a Unity project use the following folder structure:

`{Project}/Assets/Plugins/{Platform}`

## Installation
- Download [ACPCore-1.0.0-Unity.zip](https://github.com/adobe/unity-acpcore/tree/master/bin/ACPCore-1.0.0-Unity.zip) 
- Unzip `ACPCore-1.0.0-Unity.zip`
- Import `ACPCore.unitypackage` via Assets->Import Package

- Download [ACPUserProfile-1.0.0-Unity.zip](https://github.com/adobe/unity_acpuserprofile/tree/master/bin/ACPUserProfile-1.0.0-Unity.zip) 
- Unzip`ACPUserProfile-1.0.0-Unity.zip`
- Import `ACPUserProfile.unitypackage` via Assets->Import Package

#### Android installation
No additional steps are required for Android installation.

#### iOS installation
ACPCore 1.0.0 and above is shipped with XCFrameworks. Follow these steps to add them to the Xcode project generated when building and running for iOS platform in Unity.
1. Go to File -> Project Settings -> Build System and select `New Build System`.
2. [Download](https://github.com/Adobe-Marketing-Cloud/acp-sdks/tree/master/iOS/ACPCore) `ACPCore.xcframework`, `ACPIdentity.xcframework`, `ACPLifecycle.xcframework` and `ACPSignal.xcframework`.
3. [Download](https://github.com/Adobe-Marketing-Cloud/acp-sdks/tree/master/iOS/ACPUserProfile) `ACPUserProfile.xcframework`.
4. Select the UnityFramework target -> Go to Build Phases tab -> Add the XCFrameworks downloaded in Steps 2 and 3 to `Link Binary with Libraries`.
5. Select the Unity-iPhone target -> Go to Build Phases tab -> Add the XCFrameworks downloaded in Steps 2 and 3 to `Link Binary with Libraries` and `Embed Frameworks`. Alternatively, select `Unity-iPhone` target -> Go to `General` tab -> Add the XCFrameworks downloaded in Steps 2 and 3 to `Frameworks, Libraries, and Embedded Content` -> Select `Embed and sign` option.

## Usage

### [UserProfile](https://aep-sdks.gitbook.io/docs/using-mobile-extensions/profile)

#### Initialization
##### Initialize by registering the extensions and calling the start function for core
```
using com.adobe.marketing.mobile;
using AOT;

[MonoPInvokeCallback(typeof(AdobeStartCallback))]
public static void HandleStartAdobeCallback()
{
    ACPCore.ConfigureWithAppID("<appId>");    
}

public class MainScript : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {   
        if (Application.platform == RuntimePlatform.Android) {
            ACPCore.SetApplication();
        }
        
        ACPCore.SetLogLevel(ACPCore.ACPMobileLogLevel.VERBOSE);
        ACPCore.SetWrapperType();
        ACPIdentity.RegisterExtension();
        ACPUserProfile.RegisterExtension();
        ACPCore.Start(HandleStartAdobeCallback);
    }
}
```

#### UserProfile methods

##### Getting UserProfile version:
```cs
ACPUserProfile.ExtensionVersion();
```

#### Get user profile attributes which match the provided keys:
```cs
[MonoPInvokeCallback(typeof(AdobeGetUserAttributesCallback))]
public static void HandleAdobeGetUserAttributesCallback(string userAttributes)
{
    print("Attributes are : " + userAttributes);
    results = "Attributes are : " + userAttributes;
}

var attributeKeys = new List<string>(); 
attributeKeys.Add("attrNameTest");
attributeKeys.Add("mapKey");
ACPUserProfile.GetUserAttributes(attributeKeys, HandleAdobeGetUserAttributesCallback);
```

#### Remove user profile attribute if it exists:
```cs
ACPUserProfile.RemoveUserAttribute("attrNameTest");
```

#### Remove provided user profile attributes if they exist:
```cs
var attributeKeys = new List<string>(); 
attributeKeys.Add("attrNameTest");
attributeKeys.Add("mapKey");
ACPUserProfile.RemoveUserAttributes(attributeKeys);
```

#### Set a single user profile attribute:
```cs
ACPUserProfile.UpdateUserAttribute("attrNameTest", "attrValueTest");
```

#### Set multiple user profile attributes:
```cs
var dict = new Dictionary<string, object>();
dict.Add("mapKey", "mapValue");
dict.Add("mapKey1", "mapValue1");
ACPUserProfile.UpdateUserAttributes(dict);
```

## Running Tests
1. Open the demo app in unity.
2. Open the test runner from `Window -> General -> TestRunner`.
3. Click on the `PlayMode` tab.
4. Connect an Android or iOS device as we run the tests on a device in play mode.
5. Select the platform for which the tests need to be run from `File -> Build Settings -> Platform`. 
5. Click `Run all in player (platform)` to run the tests.

## Sample App
Sample App is located at *unity-acpuserprofile/ACPUserProfile/Assets/Demo*.
To build demo app for specific platform follow the below instructions.

###### Add core plugin
- Download [ACPCore-1.0.0-Unity.zip](https://github.com/adobe/unity-acpcore/tree/master/bin/ACPCore-1.0.0-Unity.zip) 
- Unzip `ACPCore-1.0.0-Unity.zip`
- Import `ACPCore.unitypackage` via Assets->Import Package

###### Android
1. Make sure you have an Android device connected.
2. From the menu of the `Unity` app, select __File > Build Settings...__
3. Select `Android` from the __Platform__ window
4. If `Android` is not the active platform, hit the button that says __Switch Platform__ (it will only be available if you actually need to switch active platforms)
5. Press the __Build And Run__ button
6. You will be asked to provide a location to save the build. Make a new directory at *unity_acpuserprofile/ACPUserProfile/Builds* (this folder is in the .gitignore file)
7. Name build whatever you want and press __Save__
8. `Unity` will build an `apk` file and automatically deploy it to the connected device

###### iOS
1. From the menu of the `Unity` app, select __File > Build Settings...__
2. Select `iOS` from the __Platform__ window
3. If `iOS` is not the active platform, hit the button that says __Switch Platform__ (it will only be available if you actually need to switch active platforms)
4. Press the __Build And Run__ button
5. You will be asked to provide a location to save the build. Make a new directory at *unity_acpuserprofile/ACPUserProfile/Builds* (this folder is in the .gitignore file)
6. Name build whatever you want and press __Save__
7. `Unity` will create and open an `Xcode` project
8. [Add XCFrameworks to the Xcode project](#ios-installation).
9. From the Xcode project run the app on a simulator.

## Additional Cordova Plugins

Below is a list of additional Unity plugins from the AEP SDK suite:

| Extension | GitHub | Unity Package |
|-----------|--------|-----|
| Core SDK | https://github.com/adobe/unity-acpcore | [ACPCore](https://github.com/adobe/unity-acpcore/blob/master/bin/ACPCore-1.0.0-Unity.zip)
| AEP Assurance | https://github.com/adobe/unity-aepassurance | [AEPAssurance](https://github.com/adobe/unity-aepassurance/blob/master/bin/AEPAssurance-1.0.0-Unity.zip)
| Analytics SDK | https://github.com/adobe/unity-acpanalytics | [ACPAnalytics](https://github.com/adobe/unity-acpanalytics/blob/master/bin/ACPAnalytics-1.0.0-Unity.zip)

## Contributing
Looking to contribute to this project? Please review our [Contributing guidelines](.github/CONTRIBUTING.md) prior to opening a pull request.

We look forward to working with you!

## Licensing
This project is licensed under the Apache V2 License. See [LICENSE](LICENSE) for more information.
