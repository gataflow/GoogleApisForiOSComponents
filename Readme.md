# Fork notes:

## Firebase Analytics

On .NET6+, FirebaseAnalytics 10.17+ requires this target be added to your app's .csproj:

```xml
<!-- Target needed until LinkWithSwiftSystemLibraries makes it into the SDK: https://github.com/xamarin/xamarin-macios/pull/20463 -->
<Target Name="LinkWithSwift" DependsOnTargets="_ParseBundlerArguments;_DetectSdkLocations" BeforeTargets="_LinkNativeExecutable">
    <PropertyGroup>
        <_SwiftPlatform Condition="$(RuntimeIdentifier.StartsWith('iossimulator-'))">iphonesimulator</_SwiftPlatform>
        <_SwiftPlatform Condition="$(RuntimeIdentifier.StartsWith('ios-'))">iphoneos</_SwiftPlatform>
    </PropertyGroup>
    <ItemGroup>
        <_CustomLinkFlags Include="-L" />
        <_CustomLinkFlags Include="/usr/lib/swift" />
        <_CustomLinkFlags Include="-L" />
        <_CustomLinkFlags Include="$(_SdkDevPath)/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/$(_SwiftPlatform)" />
        <_CustomLinkFlags Include="-Wl,-rpath" />
        <_CustomLinkFlags Include="-Wl,/usr/lib/swift" />
    </ItemGroup>
</Target>
```

On Xamarin.iOS, FirebaseAnalytics 10.17+ requires that additional mtouch arguments be added to the iOS project:

For iPhone: `--gcc_flags -L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphoneos`

For simulator: `--gcc_flags -L/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/swift/iphonesimulator/`

You may need to adjust the XCode location if it's not default.

## Firebase Crashlytics

On .NET8, you must set the following property. Note that this will increase your app's size.

```xml
<!--https://github.com/xamarin/GoogleApisForiOSComponents/issues/643#issuecomment-1920970044-->
<PropertyGroup Condition="'$(TargetFramework)' == 'net8.0-ios'">
    <_ExportSymbolsExplicitly>false</_ExportSymbolsExplicitly>
</PropertyGroup>
```

# Xamarin Components for Google APIs for iOS

Xamarin creates and maintains Xamarin.iOS bindings for the Google APIs for iOS Libraries, including:

**Active Libraries**


| Package Id                                                                 | NuGet                                      |
| -------------------------------------------------------------------------- | ------------------------------------------ |
| [Xamarin.Firebase.iOS.ABTesting][F.ABTesting.Name]                         | [10.24.0][F.ABTesting.Package]              |
| [Xamarin.Firebase.iOS.AdMob][F.AdMob.Name]                                 | [8.10.0.0][F.AdMob.Package]                |
| [AdamE.Firebase.iOS.Analytics][F.Analytics.Name]                           | [10.24.0][F.Analytics.Package]             |
| [AdamE.Firebase.iOS.Auth][F.Auth.Name]                                     | [10.24.0][F.Auth.Package]                  |
| [AdamE.Firebase.iOS.CloudFirestore][F.CloudFirestore.Name]                 | [10.24.0][F.CloudFirestore.Package]        |
| [AdamE.Firebase.iOS.CloudFunctions][F.CloudFunctions.Name]                 | [10.24.0][F.CloudFunctions.Package]        |
| [AdamE.Firebase.iOS.CloudMessaging][F.CloudMessaging.Name]                 | [10.24.0][F.CloudMessaging.Package]        |
| [AdamE.Firebase.iOS.Core][F.Core.Name]                                     | [10.24.0][F.Core.Package]                  |
| [Xamarin.Firebase.iOS.Crashlytics][F.Crashlytics.Name]                     | [10.24.0][F.Crashlytics.Package]            |
| [Xamarin.Firebase.iOS.Database][F.Database.Name]                           | [10.24.0][F.Database.Package]               |
| [Xamarin.Firebase.iOS.DynamicLinks][F.DynamicLinks.Name]                   | [10.24.0][F.DynamicLinks.Package]           |
| [Xamarin.Firebase.iOS.InAppMessaging][F.InAppMessaging.Name]               | [8.10.0][F.InAppMessaging.Package]         |
| [AdamE.Firebase.iOS.Installations][F.Installations.Name]                   | [10.24.0][F.Installations.Package]         |
| [Xamarin.Firebase.iOS.PerformanceMonitoring][F.PerformanceMonitoring.Name] | [8.10.0][F.PerformanceMonitoring.Package]  |
| [Xamarin.Firebase.iOS.RemoteConfig][F.RemoteConfig.Name]                   | [10.24.0][F.RemoteConfig.Package]           |
| [AdamE.Firebase.iOS.Storage][F.Storage.Name]                               | [10.24.0][F.Storage.Package]               |
| [Xamarin.Google.iOS.Analytics][G.Analytics.Name]                           | [3.20.0.0][G.Analytics.Package]            |
| [Xamarin.Google.iOS.Cast][G.Cast.Name]                                     | [4.7.0.0][G.Cast.Package]                  |
| [Xamarin.Google.iOS.Maps][G.Maps.Name]                                     | [6.0.1.0][G.Maps.Package]                  |
| [Xamarin.Google.iOS.MobileAds][G.MobileAds.Name]                           | [8.13.0.0][G.MobileAds.Package]            |
| [Xamarin.Google.iOS.UserMessagingPlatform][G.UserMessagingPlatform.Name]   | [1.1.0.0][G.UserMessagingPlatform.Package] |
| [Xamarin.Google.iOS.Places][G.Places.Name]                                 | [6.0.0.0][G.Places.Package]                |
| [Xamarin.Google.iOS.SignIn][G.SignIn.Name]                                 | [5.0.2.2][G.SignIn.Package]                |
| [Xamarin.Google.iOS.TagManager][G.TagManager.Name]                         | [7.4.0.0][G.TagManager.Package]            |

**Deprecated Libraries**


| Package Id                                                                   | NuGet                                        |
| ---------------------------------------------------------------------------- | -------------------------------------------- |
| [Xamarin.Firebase.iOS.InstanceID][F.InstanceID.Name]                         | [7.11.0.0][F.InstanceID.Package]             |
| [Xamarin.Google.iOS.AppIndexing][G.AppIndexing.Name]                         | [2.0.3.8][G.AppIndexing.Package]             |
| [Xamarin.Google.iOS.InstanceID][G.InstanceID.Name]                           | [1.2.1.18][G.InstanceID.Package]             |
| [Xamarin.Google.iOS.PlayGames][G.PlayGames.Name]                             | [5.1.1.11][G.PlayGames.Package]              |
| [Xamarin.Firebase.iOS.CrashReporting][F.CrashReporting.Name]                 | [2.0.0.6][F.CrashReporting.Package]          |
| [Xamarin.Firebase.iOS.Invites][F.Invites.Name]                               | [3.0.1.1][F.Invites.Package]                 |
| [Xamarin.Google.iOS.AppInvite][G.AppInvite.Name]                             | [1.0.2.4][G.AppInvite.Package]               |
| [Xamarin.Google.iOS.Core][G.Core.Name]                                       | [3.1.0.1][G.Core.Package]                    |
| [Xamarin.Google.iOS.GoogleCloudMessaging][G.GoogleCloudMessaging.Name]       | [1.2.0.1][G.GoogleCloudMessaging.Package]    |
| [Xamarin.Firebase.iOS.MLKit][F.MLKit.Name]                                   | [0.21.0.0][F.MLKit.Package]                  |
| [Xamarin.Firebase.iOS.MLKit.Common][F.MLKit.Common.Name]                     | [0.21.0.0][F.MLKit.Common.Package]           |
| [Xamarin.Firebase.iOS.MLKit.ModelInterpreter][F.MLKit.ModelInterpreter.Name] | [0.21.0.0][F.MLKit.ModelInterpreter.Package] |
| [Xamarin.Firebase.iOS.MLKit.NaturalLanguage][F.MLKit.NaturalLanguage.Name]   | [0.18.1.0][F.MLKit.NaturalLanguage.Package]  |
| [Xamarin.Firebase.iOS.MLKit.Vision][F.MLKit.Vision.Name]                     | [0.21.0.0][F.MLKit.Vision.Package]           |

## Firebase APIs for iOS current global version

Here's a table that shows in which global version is located each component of Firebase at this point of history:


| Component Name                  | Component Version |   Global Version   |
| ------------------------------- | :---------------: | :----------------: |
| Firebase A/B Testing            |    **10.24.0**    | **10.24.0-alpha1** |
| Firebase AdMob                  |    **8.10.0**     |     **8.10.0**     |
| Firebase Analytics              |    **10.24.0**    |    **10.24.0**     |
| Firebase Auth                   |    **10.24.0**    |    **10.24.0**     |
| Firebase Cloud Firestore        |    **10.24.0**    |    **10.24.0**     |
| Firebase Cloud Functions        |    **10.24.0**    |    **10.24.0**     |
| Firebase Cloud Messaging        |    **10.24.0**    |    **10.24.0**     |
| Firebase Core                   |    **10.24.0**    |    **10.24.0**     |
| Firebase Crashlytics            |    **10.24.0**    |    **10.24.0**     |
| Firebase Database               |    **10.24.0**    | **10.24.0-alpha1** |
| Firebase Dynamic Links          |    **8.10.0**     | **10.24.0-alpha1** |
| Firebase In App Messaging       |    **8.10.0**     |     **8.10.0**     |
| Firebase Installations          |    **10.24.0**    |    **10.24.0**     |
| Firebase Performance Monitoring |    **8.10.0**     |     **8.10.0**     |
| Firebase RemoteConfig           |    **8.10.0**     | **10.24.0-alpha1** |
| Firebase Storage                |    **10.24.0**    |    **10.24.0**     |
| Google User Messaging Platform  |    **1.1.0.0**    |     **8.10.0**     |
| Google Cast                     |    **4.7.0.0**    |     **8.10.0**     |
| Google Sign-In                  |    **5.0.2.2**    |     **8.10.0**     |
| Google Tag Manager              |    **7.4.0.0**    |     **8.10.0**     |

## Ad Id Support

By default Firebase includes Ad Id Support, however, it can be disabled by adding the below property group to your project file.

```xml
<PropertyGroup>
  <FirebaseWithoutAdIdSupport>True</FirebaseWithoutAdIdSupport>
</PropertyGroup>
```

## Building

### Prerequisites

Before building the libraries and samples in this repository, you will need to install [.NET Core][30] and the [Cake .NET Core Tool][32]:

Currently requires a version of Cake less than 1.0 (due to dependencies).

```sh
dotnet tool install -g cake.tool --version 0.38.5
```

When building on macOS, you may also need to install [CocoaPods][31]:

```sh
# Homebrew
brew install cocoapods

# Ruby Gems
gem install cocoapods
```

### Compiling

You can either build all the libraries and samples in the repository from the root:

```sh
dotnet cake
```

Or, you can specify the components and its dependencies to be build by using the `--names=Key1,Key2,...`:

```sh
// Firebase keys
Firebase.ABTesting
Firebase.AdMob
Firebase.Analytics
Firebase.Auth
Firebase.CloudFirestore
Firebase.CloudFunctions
Firebase.CloudMessaging
Firebase.Core
Firebase.Crashlytics
Firebase.Database
Firebase.DynamicLinks
Firebase.InAppMessaging
Firebase.Installations
Firebase.PerformanceMonitoring
Firebase.RemoteConfig
Firebase.Storage

// Google keys
Google.Analytics
Google.Cast
Google.Maps
Google.MobileAds
Google.UserMessagingPlatform
Google.Places
Google.SignIn
Google.TagManager

// MLKit keys
MLKit.BarcodeScanning
MLKit.Core
MLKit.DigitalInkRecognition
MLKit.FaceDetection
MLKit.ImageLabeling
MLKit.ObjectDetection
MLKit.TextRecognition
MLKit.TextRecognition.Chinese
MLKit.TextRecognition.Devanagari
MLKit.TextRecognition.Japanese
MLKit.TextRecognition.Korean
MLKit.TextRecognition.Latin
MLKit.Vision
```

The following targets can be specified using the `--target=<target-name>`:

- `libs` builds the class library bindings (depends on `externals`)
- `externals` downloads and builds the external dependencies
- `samples` builds all of the samples (depends on `libs`)
- `nuget` builds the nuget packages (depends on `libs`)
- `clean` cleans up everything

### Working in Visual Studio

Before the `.sln` files will compile in the IDEs, the external dependencies need to be downloaded. This can be done by running the `externals` target:

```sh
dotnet cake --target=externals
```

After the externals are downloaded and built, the `.sln` files should compile in your IDE.

## License

The license for this repository is specified in
[License.md](License.md)

## Contribution Guidelines

You will need to complete a Contribution License Agreement before your pull request can be accepted. You can complete the CLA by going through the steps at [https://cla2.dotnetfoundation.org/][103].

## .NET Foundation

This project is part of the [.NET Foundation][104]

[comment]: #
[F.ABTesting.Name]: source/Firebase/ABTesting
[F.AdMob.Name]: source/Firebase/AdMob
[F.Analytics.Name]: source/Firebase/Analytics
[F.Auth.Name]: source/Firebase/Auth
[F.CloudFirestore.Name]: source/Firebase/CloudFirestore
[F.CloudFunctions.Name]: source/Firebase/CloudFunctions
[F.CloudMessaging.Name]: source/Firebase/CloudMessaging
[F.Core.Name]: source/Firebase/Core
[F.Crashlytics.Name]: source/Firebase/Crashlytics
[F.Database.Name]: source/Firebase/Database
[F.DynamicLinks.Name]: source/Firebase/DynamicLinks
[F.InAppMessaging.Name]: source/Firebase/InAppMessaging
[F.Installations.Name]: source/Firebase/Installations
[F.PerformanceMonitoring.Name]: source/Firebase/PerformanceMonitoring
[F.RemoteConfig.Name]: source/Firebase/RemoteConfig
[F.Storage.Name]: source/Firebase/Storage
[comment]: #
[F.ABTesting.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.ABTesting/
[F.AdMob.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.AdMob/
[F.Analytics.Package]: https://www.nuget.org/packages/AdamE.Firebase.iOS.Analytics/
[F.Auth.Package]: https://www.nuget.org/packages/AdamE.Firebase.iOS.Auth/
[F.CloudFirestore.Package]: https://www.nuget.org/packages/AdamE.Firebase.iOS.CloudFirestore/
[F.CloudFunctions.Package]: https://www.nuget.org/packages/AdamE.Firebase.iOS.Functions/
[F.CloudMessaging.Package]: https://www.nuget.org/packages/AdamE.Firebase.iOS.CloudMessaging/
[F.Core.Package]: https://www.nuget.org/packages/AdamE.Firebase.iOS.Core/
[F.Crashlytics.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.Crashlytics/
[F.Database.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.Database/
[F.DynamicLinks.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.DynamicLinks/
[F.InAppMessaging.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.InAppMessaging/
[F.Installations.Package]: https://www.nuget.org/packages/AdamE.Firebase.iOS.Installations/
[F.PerformanceMonitoring.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.PerformanceMonitoring/
[F.RemoteConfig.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.RemoteConfig/
[F.Storage.Package]: https://www.nuget.org/packages/AdamE.Firebase.iOS.Storage/
[comment]: #
[G.Analytics.Name]: source/Google/Analytics
[G.Cast.Name]: source/Google/Cast
[G.Maps.Name]: source/Google/Maps
[G.MobileAds.Name]: source/Google/MobileAds
[G.UserMessagingPlatform.Name]: source/Google/UserMessagingPlatform
[G.Places.Name]: source/Google/Places
[G.SignIn.Name]: source/Google/SignIn
[G.TagManager.Name]: source/Google/TagManager
[comment]: #
[G.Analytics.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.Analytics/
[G.Cast.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.Cast/
[G.Maps.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.Maps/
[G.MobileAds.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.MobileAds/
[G.UserMessagingPlatform.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.UserMessagingPlatform/
[G.Places.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.Places/
[G.SignIn.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.SignIn/
[G.TagManager.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.TagManager/
[comment]: #
[MLK.BarcodeScanning]: source/MLKit/BarcodeScanning
[MLK.Core]: source/MLKit/Core
[MLK.DigitalInkRecognition]: source/MLKit/DigitalInkRecognition
[MLK.FaceDetection]: source/MLKit/FaceDetection
[MLK.ImageLabeling]: source/MLKit/ImageLabeling
[MLK.ObjectDetection]: source/MLKit/ObjectDetection
[MLK.TextRecognition]: source/MLKit/TextRecognition
[MLK.TextRecognition.Chinese]: source/MLKit/TextRecognitionChinese
[MLK.TextRecognition.Devanagari]: source/MLKit/TextRecognitionDevanagari
[MLK.TextRecognition.Japanese]: source/MLKit/TextRecognitionJapanese
[MLK.TextRecognition.Korean]: source/MLKit/TextRecognitionKorean
[MLK.TextRecognition.Latin]: source/MLKit/TextRecognitionLatin
[MLK.Vision]: source/MLKit/Vision
[comment]: #
[F.InstanceID.Name]: source/Firebase/InstanceID
[F.CrashReporting.Name]: source/Firebase/CrashReporting
[F.Invites.Name]: source/Firebase/Invites
[F.MLKit.Name]: source/Firebase/MLKit
[F.MLKit.Common.Name]: source/Firebase/MLKit.Common
[F.MLKit.ModelInterpreter.Name]: source/Firebase/MLKit.ModelInterpreter
[F.MLKit.NaturalLanguage.Name]: source/Firebase/MLKit.NaturalLanguage
[F.MLKit.Vision.Name]: source/Firebase/MLKit.Vision
[comment]: #
[F.InstanceID.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.InstanceID/
[F.CrashReporting.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.CrashReporting/
[F.Invites.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.Invites/
[F.MLKit.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.MLKit/
[F.MLKit.Common.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.MLKit.Common/
[F.MLKit.ModelInterpreter.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.MLKit.ModelInterpreter/
[F.MLKit.NaturalLanguage.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.MLKit.NaturalLanguage/
[F.MLKit.Vision.Package]: https://www.nuget.org/packages/Xamarin.Firebase.iOS.MLKit.Vision/
[comment]: #
[G.AppIndexing.Name]: source/Google/AppIndexing
[G.InstanceID.Name]: source/Google/InstanceID
[G.PlayGames.Name]: source/Google/PlayGames
[G.AppInvite.Name]: source/Google/AppInvite
[G.Core.Name]: source/Google/Core
[G.GoogleCloudMessaging.Name]: source/Google/GoogleCloudMessaging
[comment]: #
[G.AppIndexing.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.AppIndexing/
[G.InstanceID.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.InstanceID/
[G.PlayGames.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.PlayGames/
[G.AppInvite.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.AppInvite/
[G.Core.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.Core/
[G.GoogleCloudMessaging.Package]: https://www.nuget.org/packages/Xamarin.Google.iOS.GoogleCloudMessaging/
[101]: https://cocoapods.org/
[102]: http://cakebuild.net
[103]: https://cla2.dotnetfoundation.org/
[104]: http://www.dotnetfoundation.org/projects
[30]: https://dotnet.microsoft.com/download
[31]: https://cocoapods.org/
[32]: http://cakebuild.net
[33]: https://cla2.dotnetfoundation.org/
[34]: http://www.dotnetfoundation.org/projects
