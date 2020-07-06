* [xcode-select]()
* [xcodebuild]()
* [xcrun]()

|命令|含义|
| --- | --- |
| `-usage` | |
| `-help` | |
| `-verbose` | |
| `-license` | |
| `-checkFirstLaunchStatus` | |
| `-runFirstLaunch` | |
| `-project NAME` | |
| `-target NAME` | |
| `-alltargets` | |
| `-workspace NAME` | |
| `-scheme NAME` | |
| `-configuration NAME` | |
| `-xcconfig PATH` | |
| `-arch ARCH` | |
| `-sdk SDK` | |
| `-toolchain NAME` | |
| `-destination DESTINATIONSPECIFIER` | |
| `-destination-timeout TIMEOUT` | |
| `-parallelizeTargets` | |
| `-jobs NUMBER` | |
| `-maximum-concurrent-test-device-destinations NUMBER` | |
| `-maximum-concurrent-test-simulator-destinations NUMBER` | |
| `-parallel-testing-enabled YES/NO` | |
| `-parallel-testing-worker-count NUMBER` | |
| `-maximum-parallel-testing-workers NUMBER` | |
| `-dry-run` | |
| `-quiet` | |
| `-hideShellScriptEnvironment` | |
| `-showsdks` | |
| `-showdestinations` | |
| `-showTestPlans` | |
| `-showBuildSettings` | |
| `-showBuildSettingsForIndex` | |
| `-list` | |
| `-find-executable NAME` | |
| `-find-library NAME` | |
| `-version` | |
| `-enableAddressSanitizer YES/NO` | |
| `-enableThreadSanitizer YES/NO` | |
| `-enableUndefinedBehaviorSanitizer YES/NO` | |
| `-resultBundlePath PATH` | |
| `-resultStreamPath PATH` | |
| `-resultBundleVersion 3 [default]` | |
| `-clonedSourcePackagesDirPath PATH` | |
| `-derivedDataPath PATH` | |
| `-archivePath PATH` | |
| `-exportArchive` | |
| `-exportNotarizedApp` | |
| `-exportOptionsPlist PATH` | |
| `-enableCodeCoverage YES/NO` | |
| `-exportPath PATH` | |
| `-skipUnavailableActions` | |
| `-exportLocalizations` | |
| `-importLocalizations` | |
| `-localizationPath` | |
| `-exportLanguage` | |
| `-xcroot` | |
| `-xctestrun` | |
| `-testPlan` | |
| `-only-testing` | |
| `-only-testing:TEST-IDENTIFIER` | |
| `-skip-testing` | |
| `-skip-testing:TEST-IDENTIFIER` | |
| `-test-timeouts-enabled YES/NO` | |
| `-default-test-execution-time-allowance` | |
| `-maximum-test-execution-time-allowance` | |
| `-only-test-configuration` | |
| `-skip-test-configuration` | |
| `-testLanguage` | |
| `-testRegion` | |
| `-resolvePackageDependencies` | |
| `-disableAutomaticPackageResolution` | |
| `-json` | |
| `-allowProvisioningUpdates` | |
| `-allowProvisioningDeviceRegistration` | |
| `-showBuildTimingSummary` | |
| `-create-xcframework` | |

# 查看版本号

```shell
% xcodebuild -version
Xcode 11.5
Build version 11E608c
```

# 查看已安装的SDK

```shell
% xcodebuild -showsdks
iOS SDKs:
	iOS 13.5                      	-sdk iphoneos13.5

iOS Simulator SDKs:
	Simulator - iOS 13.5          	-sdk iphonesimulator13.5

macOS SDKs:
	DriverKit 19.0                	-sdk driverkit.macosx19.0
	macOS 10.15                   	-sdk macosx10.15

tvOS SDKs:
	tvOS 13.4                     	-sdk appletvos13.4

tvOS Simulator SDKs:
	Simulator - tvOS 13.4         	-sdk appletvsimulator13.4

watchOS SDKs:
	watchOS 6.2                   	-sdk watchos6.2

watchOS Simulator SDKs:
	Simulator - watchOS 6.2       	-sdk watchsimulator6.2
```

## 查看安装SDK的详细地址

```shell
% xcodebuild -sdk iphonesimulator13.5
Command line invocation:
    /Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild -sdk iphonesimulator13.5

Build settings from command line:
    SDKROOT = iphonesimulator13.5

xcodebuild: error: The directory /usr/bin does not contain an Xcode project.
```

# 资料
* [Xcodebuild从入门到精通](https://www.hualong.me/2018/03/14/Xcodebuild/)

# Options

