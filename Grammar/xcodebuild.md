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

## -project

```sh
-project name.xcodeproj
# Build the project name.xcodeproj.  Required if there are multiple project files in the same directory.
```

## -workspace

```sh
-workspace name.xcworkspace
# Build the workspace name.xcworkspace.
```

## -target

```sh
-target targetname
# Build the target specified by targetname.
```

## -alltargets

```sh
-alltargets
# Build all the targets in the specified project.
```

## -scheme

```sh
-scheme schemename
# Build the scheme specified by schemename.  Required if building a workspace
```

## -destination

```sh
-destination destinationspecifier
# Use the destination device described by destinationspecifier.  Defaults to a destination that is compatible with the selected scheme.  See the Destinations section below for more details.
```

## -destination-timeout

```sh
-destination-timeout timeout
# Use the specified timeout when searching for a destination device. The default is 30 seconds.
```

## -configuration

```sh
-configuration configurationname
# Use the build configuration specified by configurationname when building each target.
```

## -arch

```sh
-arch architecture
# Use the architecture specified by architecture when building each target.
```

## -sdk

```sh
-sdk [sdkfullpath | sdkname]
# Build an Xcode project or workspace against the specified SDK, using build tools appropriate for that SDK. The argument may be an absolute path to an SDK, or the canonical name of an SDK.
```

## -showsdks

```sh
-showsdks
# Lists all available SDKs that Xcode knows about, including their canonical names suitable for use with -sdk.  Does not initiate a build.
```

## -showBuildSettings

```sh
-showBuildSettings
# Lists the build settings in a project or workspace and scheme. Does not initiate a build. Use with -project or -workspace and -scheme.
```

## -showdestinations

```sh
-showdestinations
# Lists the valid destinations for a project or workspace and scheme. Does not initiate a build. Use with -project or -workspace and -scheme.
```

## -showBuildTimingSummary

```sh
-showBuildTimingSummary
# Display a report of the timings of all the commands invoked during the build.
```

## -showTestPlans

```sh
-showTestPlans
# Lists the test plans (if any) associated with the specified scheme. Does not initiate a build. Use with -scheme.
```

## -list

```sh
-list
# Lists the targets and configurations in a project, or the schemes in a workspace. Does not initiate a build. Use with -project or -workspace.
```

## -enableAddressSanitizer

```sh
-enableAddressSanitizer [YES | NO]
# Turns the address sanitizer on or off. This overrides the setting for the launch action of a scheme in a workspace.
```

## -enableThreadSanitizer

```sh
-enableThreadSanitizer [YES | NO]
# Turns the thread sanitizer on or off. This overrides the setting for the launch action of a scheme in a workspace.
```

## -enableUndefinedBehaviorSanitizer

```sh
-enableUndefinedBehaviorSanitizer [YES | NO]
# Turns the undefined behavior sanitizer on or off. This overrides the setting for the launch action of a scheme in a workspace.
```

## -enableCodeCoverage

```sh
-enableCodeCoverage [YES | NO]
# Turns code coverage on or off during testing. This overrides the setting for the test action of a scheme in a workspace.
```

## -testLanguage

```sh
-testLanguage language
# Specifies ISO 639-1 language during testing. This overrides the setting for the test action of a scheme in a workspace.
```

## -testRegion

```sh
-testRegion region
# Specifies ISO 3166-1 region during testing. This overrides the setting for the test action of a scheme in a workspace.
```

## -derivedDataPath

```sh
-derivedDataPath path
# Overrides the folder that should be used for derived data when performing an action on a scheme in a workspace.

```

# Destinations

当前`xcodebuild`支持的平台为：

* `macOS`
* `iOS`
* `iOS Simulator`
* `watchOS`
* `watchOS Simulator`
* `tvOS`
* `tvOS Simulator`













