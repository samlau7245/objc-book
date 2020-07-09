* [xcode-select]()
* [xcodebuild]()
* [xcrun]()

<img src="/assets/images/02.png"/>

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
| `-arch ARCH` | 指定 指令集|
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

# Destinations

`Destinations`用来指定可用设备：

* `platform`:
	* `macOS` :
		* `arch` : 指定只用的指令集。`x86_64`、`i386`。
		* `variant` : `Mac`、`Catalyst`-把iPad应用一键移到Mac。
	* `iOS` : 
		* `id` : 需要使用设备的UDID，id和name两者择其一。
		* `name` : 需要使用设备的NAME，id和name两者择其一。
	* `iOS Simulator` : 
		* `id` : 需要使用设备的UDID，id和name两者择其一。
		* `name` : 虚拟机的名字，id和name两者择其一。	
		* `OS` : 虚拟机的系统，默认最新`latest`。
	* `watchOS` : 
	* `watchOS Simulator` : 
	* `tvOS` : 
		* `id` : 需要使用设备的UDID，id和name两者择其一。
		* `name` : 需要使用设备的NAME，id和name两者择其一。
	* `tvOS Simulator` : 
		* `id` : 需要使用设备的UDID，id和name两者择其一。
		* `name` : 虚拟机的名字，id和name两者择其一。	
		* `OS` : 虚拟机的系统，默认最新`latest`。

# Options

## 所有Xcode可用SDK

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

## -list 查看项目中的目标和配置

```sh
% xcodebuild -list                          
Command line invocation:
    /Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild -list

2020-07-09 16:53:31.698 xcodebuild[30453:2382605]  DTDeviceKit: deviceType from b21b54f8a8c80bc625538ce60e405aa060147a09 was NULL
2020-07-09 16:53:31.760 xcodebuild[30453:2382715]  DTDeviceKit: deviceType from b21b54f8a8c80bc625538ce60e405aa060147a09 was NULL
Information about project "XcodebuildExample":
    Targets:
        XcodebuildExample

    Build Configurations:
        Debug
        Release

    If no build configuration is specified and -scheme is not passed then "Release" is used.

    Schemes:
        XcodebuildExample
```

## -destination 指定目标设备

> * `-destination destinationspecifier`：指定设备。
> * `-destination-timeout timeout`: 指定设备的超时时间，默认30秒。

```sh
# 指定设备为：系统 iOS11.2 iPhone6s 模拟器。
-destination 'platform=iOS Simulator,name=iPhone 6s,OS=11.2'
```

## -configuration 构建方式

> 通过`-list` 可以看到对应项目的`configuration`。

<img src="/assets/images/03.png"/>

```sh
-configuration Debug
# 或
-configuration Release
```

## 列出Target中所有Build settings

> `xcodebuild -showBuildSettings`

<img src="/assets/images/04.png"/>

```sh
% xcodebuild -showBuildSettings
Command line invocation:
    /Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild -showBuildSettings

2020-07-09 17:32:52.576 xcodebuild[33081:2408968]  DTDeviceKit: deviceType from b21b54f8a8c80bc625538ce60e405aa060147a09 was NULL
2020-07-09 17:32:52.633 xcodebuild[33081:2408964]  DTDeviceKit: deviceType from b21b54f8a8c80bc625538ce60e405aa060147a09 was NULL
Build settings for action build and target XcodebuildExample:
    ACTION = build
    AD_HOC_CODE_SIGNING_ALLOWED = NO
    ALTERNATE_GROUP = staff
    ALTERNATE_MODE = u+w,go-w,a+rX
    ALTERNATE_OWNER = shanliu
    ALWAYS_EMBED_SWIFT_STANDARD_LIBRARIES = NO
    ALWAYS_SEARCH_USER_PATHS = NO
    ALWAYS_USE_SEPARATE_HEADERMAPS = NO
    APPLE_INTERNAL_DEVELOPER_DIR = /AppleInternal/Developer
    APPLE_INTERNAL_DIR = /AppleInternal
    省略....
```

[Xcode Help-Build Settings](https://help.apple.com/xcode/mac/current/#/itcaec37c2a6)

## -enableThreadSanitizer

> `-enableThreadSanitizer [YES|NO]`

<img src="/assets/images/05.png"/>

## -enableThreadSanitizer

`-enableThreadSanitizer [YES|NO]`

<img src="/assets/images/06.png"/>

## -enableCodeCoverage

> `-enableCodeCoverage [YES|NO]`

<img src="/assets/images/07.png"/>

## test使用的语言

> `-testLanguage language`：用`ISO 3166-1`语言名称指定APP所用语言。

<img src="/assets/images/08.png"/>

## test的区域

> `-testRegion region`：用`ISO 3166-1`区域名称指定APP所在区域。

<img src="/assets/images/09.png"/>

## -allowProvisioningUpdates

> 允许`xcodebuild`与`Apple Developer`网站进行通信。对于自动签名的目标，`xcodebuild`将创建并更新配置文件，应用程序ID和证书。 对于手动签名的目标，`xcodebuild`将下载缺失或更新的供应配置文件， 需要在Xcode的帐户首选项窗格中添加开发者帐户。

## 导出IPA文件

* `-exportArchive` : 导出IPA文件；需要与`-archivePath`，`-exportPath`和 `-exportOptionsPlist`一起使用。
* `-archivePath xcarchivepath` : 指定archive操作生成归档的路径。或者在使用`-exportArchive`时指定归档的路径。
* `-exportPath destinationpath` : 指定导出IPA文件到哪个路径。
* `-exportOptionsPlist path` : 指定`ExportOptions.plist`路径。导出IPA文件时，需要指定一个`ExportOptions.plist`文件。
* `-exportNotarizedApp` : 需要 `-archivePath` 和 `-exportPath`。
* `-exportLocalizations` : 将本地化导出到XLIFF文件。需要`-project`和`-localizationPath`。
* `-exportLanguage language` : 指定包含在本地化导出中的可选`ISO 639-1`语言。

# actions

* `build`
* `build-for-testing`
* `analyze`
* `archive`
* `test`
* `test-without-building`
* `installsrc`
* `install`
* `clean`

# 示例


# 资料
* [Xcodebuild从入门到精通](https://www.hualong.me/2018/03/14/Xcodebuild/)
























