* [xcode-select]()
* [xcodebuild](): 负责编译
* [xcrun](): 负责将app打成ipa

<img src="/assets/images/02.png"/>

|命令|含义|
| --- | --- |
| `-usage` | 查看xcodebuild简洁的用法 |
| `-help` | 查看帮助 |
| `-verbose` | 提供额外的状态输出 |
| `-license` | 显示Xcode和SDK许可协议 |
| `-checkFirstLaunchStatus` | 检查是否需要执行首次启动任务 |
| `-runFirstLaunch` | 安装软件包并同意许可证 |
| `-project NAME` | 编译项目名称，例如:`xcodebuild -project XXX.xcodeproj` |
| `-target NAME` | 编译目标名称 |
| `-alltargets` | |
| `-workspace NAME` | NAME编译工作空间名称 |
| `-scheme NAME` | 编译计划名称 |
| `-configuration NAME` | 为构建每一个目标使用`build`配置名称 |
| `-xcconfig PATH` | 在`PATH`作为替代应用文件中定义的构建设置 |
| `-arch ARCH` | 指定 指令集 `arm64 armv7 armv7s`|
| `-sdk SDK` | 使用指定的SDK编译项目 `-sdk iphoneos`|
| `-toolchain NAME` | 使用identifier或name指定的toolchain |
| `-destination DESTINATIONSPECIFIER` | |
| `-destination-timeout TIMEOUT` | |
| `-parallelizeTargets` | 建立并行独立目标 |
| `-jobs NUMBER` | 指定并发生成操作的最大数量 |
| `-maximum-concurrent-test-device-destinations NUMBER` | 限制最多多少台真实设备并发测试 |
| `-maximum-concurrent-test-simulator-destinations NUMBER` | 限制最多多少台并发测试 |
| `-parallel-testing-enabled YES/NO` | |
| `-parallel-testing-worker-count NUMBER` | |
| `-maximum-parallel-testing-workers NUMBER` | |
| `-dry-run` | 打印将执行的命令，但不执行它们。 |
| `-quiet` | 除了警告和错误外，不打印任何输出 |
| `-hideShellScriptEnvironment` | |
| `-showsdks` | 显示已安装的SDK的列表 |
| `-showdestinations` | |
| `-showTestPlans` | |
| `-showBuildSettings` | 显示构建设置和值的列表 |
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
| `-skipUnavailableActions` | 跳过无法执行的操作而不是失败的操作。 这个选项是只有在scheme通过的情况下才会被使用。 |
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
| `-json` | 输出为JSON格式 |
| `-allowProvisioningUpdates` | |
| `-allowProvisioningDeviceRegistration` | 如有必要，允许xcodebuild在Apple Developer网站上注册您的目标设备。需要`-allowProvisioningUpdates`。 |
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

## -showBuildTimingSummary

<img src="/assets/images/11.png"/>

显示编译过程中所有命令的执行时间。

## -showTestPlans

```sh
xcodebuild -showTestPlans [-project name.xcodeproj | -workspace name.xcworkspace] -scheme schemename
```

```sh
% xcodebuild -showTestPlans
# xcodebuild: error: -showTestPlans requires a workspace and scheme.

% xcodebuild -showTestPlans -workspace XcodebuildExample.xcworkspace -scheme XcodebuildExample    
# The scheme "XcodebuildExample" is not configured to use test plans.
```
## -showdestinations

```sh
xcodebuild -showdestinations [-project name.xcodeproj | [-workspace name.xcworkspace -scheme schemename]]
```

> 仅对`workspace`有效。

```sh
% xcodebuild -showdestinations -project XcodebuildExample.xcodeproj 
# xcodebuild: error: -showdestinations requires a workspace and scheme.

% xcodebuild -showdestinations -workspace XcodebuildExample.xcworkspace -scheme XcodebuildExample -json
# Available destinations for the "XcodebuildExample" scheme:
#     { platform:iOS Simulator, id:1726D99B-47E1-4A43-A520-D61FB8874DC0, OS:13.5, name:iPad (7th generation) }
#     { platform:iOS Simulator, id:71AD7AC0-60B5-4898-A46E-9A5AE4D25B9A, OS:13.5, name:iPad Air (3rd generation) }
#     { platform:iOS Simulator, id:F86FD67C-DBD4-4DBF-84F9-9836FA46FC79, OS:13.5, name:iPad Pro (9.7-inch) }
#     { platform:iOS Simulator, id:4A7C7C0D-F377-4E2F-AAC1-A6963A9049B0, OS:13.5, name:iPad Pro (11-inch) (2nd generation) }
#     { platform:iOS Simulator, id:3353A0A9-C15A-44EE-A331-E68554979DE4, OS:13.5, name:iPad Pro (12.9-inch) (4th generation) }
#     { platform:iOS Simulator, id:5E6BC9F8-DC22-4EA9-8C9B-34420BBFB1EA, OS:13.5, name:iPhone 8 }
#     { platform:iOS Simulator, id:B7842959-4C7E-4607-BDF8-34F68D1ED0B2, OS:13.5, name:iPhone 8 Plus }
#     { platform:iOS Simulator, id:859073C6-9D6B-4451-94A2-6625CC56AFC1, OS:13.5, name:iPhone 11 }
#     { platform:iOS Simulator, id:1C01DA7D-E641-43AF-AD79-D57F8B3F0D5D, OS:13.5, name:iPhone 11 Pro }
#     { platform:iOS Simulator, id:715164B6-C2C5-4354-BD28-480873D2FE10, OS:13.5, name:iPhone 11 Pro Max }
#     { platform:iOS Simulator, id:3F1AF58A-A587-4718-9B84-76EB9880518F, OS:13.5, name:iPhone SE (2nd generation) }

# Ineligible destinations for the "XcodebuildExample" scheme:
#     { platform:iOS, id:dvtdevice-DVTiPhonePlaceholder-iphoneos:placeholder, name:Generic iOS Device }
#     { platform:iOS Simulator, id:dvtdevice-DVTiOSDeviceSimulatorPlaceholder-iphonesimulator:placeholder, name:Generic iOS Simulator Device }
```

## 所有Xcode可用SDK

```shell
% xcodebuild -showsdks
iOS SDKs:
	iOS 13.5                      	-sdk iphoneos13.5

iOS Simulator SDKs:
	Simulator - iOS 13.5             	-sdk iphonesimulator13.5

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

```sh
% xcodebuild -showsdks -json
[
  {
    "buildID" : "F54BBE0A-8C9B-11EA-A993-60C1727FB22C",
    "canonicalName" : "iphoneos13.5",
    "displayName" : "iOS 13.5",
    "isBaseSdk" : true,
    "platform" : "iphoneos",
    "platformPath" : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform",
    "platformVersion" : "13.5",
    "productBuildVersion" : "17F65",
    "productCopyright" : "1983-2020 Apple Inc.",
    "productName" : "iPhone OS",
    "productVersion" : "13.5",
    "sdkPath" : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS13.5.sdk",
    "sdkVersion" : "13.5"
  },
  {
    "buildID" : "F54BBE0A-8C9B-11EA-A993-60C1727FB22C",
    "canonicalName" : "iphonesimulator13.5",
    "displayName" : "Simulator - iOS 13.5",
    "isBaseSdk" : true,
    "platform" : "iphonesimulator",
    "platformPath" : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform",
    "platformVersion" : "13.5",
    "productBuildVersion" : "17F65",
    "productCopyright" : "1983-2020 Apple Inc.",
    "productName" : "iPhone OS",
    "productVersion" : "13.5",
    "sdkPath" : "/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator13.5.sdk",
    "sdkVersion" : "13.5"
  }
  ....
]
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
xcodebuild -list [-project name.xcodeproj | -workspace name.xcworkspace]
```

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

```sh
% xcodebuild -list -json
2020-07-13 13:41:58.281 xcodebuild[59790:342617]  DTDeviceKit: deviceType from b21b54f8a8c80bc625538ce60e405aa060147a09 was NULL
2020-07-13 13:41:58.372 xcodebuild[59790:342712]  DTDeviceKit: deviceType from b21b54f8a8c80bc625538ce60e405aa060147a09 was NULL
{
  "project" : {
    "configurations" : [
      "Debug",
      "Release"
    ],
    "name" : "XcodebuildExample",
    "schemes" : [
      "XcodebuildExample"
    ],
    "targets" : [
      "XcodebuildExample"
    ]
  }
}
```

## -destination 给特定的设备进行构建

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

```sh
% xcodebuild -showBuildSettings -json
[
  {
    "action" : "build",
    "buildSettings" : {
      "ACTION" : "build",
      "AD_HOC_CODE_SIGNING_ALLOWED" : "NO",
      "ALTERNATE_GROUP" : "staff",
      "ALTERNATE_MODE" : "u+w,go-w,a+rX",
      "ALTERNATE_OWNER" : "shanliu",
      "ALWAYS_EMBED_SWIFT_STANDARD_LIBRARIES" : "NO",
      "ALWAYS_SEARCH_USER_PATHS" : "NO",
      "ALWAYS_USE_SEPARATE_HEADERMAPS" : "NO",
      "APPLE_INTERNAL_DEVELOPER_DIR" : "/AppleInternal/Developer",
      "APPLE_INTERNAL_DIR" : "/AppleInternal",
      "APPLE_INTERNAL_DOCUMENTATION_DIR" : "/AppleInternal/Documentation",
      "APPLE_INTERNAL_LIBRARY_DIR" : "/AppleInternal/Library",
      "APPLE_INTERNAL_TOOLS" : "/AppleInternal/Developer/Tools",
      "APPLICATION_EXTENSION_API_ONLY" : "NO",
      "APPLY_RULES_IN_COPY_FILES" : "NO",
      "APPLY_RULES_IN_COPY_HEADERS" : "NO",
      ......
    },
    "target" : "XcodebuildExample"
  }
]
```

[Xcode Help-Build Settings](https://help.apple.com/xcode/mac/current/#/itcaec37c2a6)

## -enableAddressSanitizer

> `-enableAddressSanitizer [YES|NO]`

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

## archive

```sh
# archive 项目
xcodebuild archive -workspace MyWorkspace.xcworkspace -scheme MyScheme [-configuration < Debug|Release>] [-sdk <sdkName>]
xcodebuild archive -project MyProject.xcodeproj -scheme MyScheme [-configuration < Debug|Release>] [-sdk <sdkName>]

# 导出 archive
xcodebuild -exportArchive -archivePath MyMobileApp.xcarchive -exportPath ExportDestination -exportOptionsPlist 'export.plist'
```

# ExportOptions.plist

* `compileBitcode`: - `Bool` 对于非App Store的出口，应重新编译Xcode中从bitcode应用程序？默认为YES。
* `embedOnDemandResourcesAssetPacksInBundle` : - `Bool`  对于非App Store的出口，如果应用程序使用按需的资源，这是YES，资产包被嵌入在应用程序包，使应用程序可以在没有服务器托管资产包进行测试。默认为YES除非指定`onDemandResourcesAssetPacksBaseURL`
* `iCloudContainerEnvironment`: 对于非App Store的出口，如果应用程序使用CloudKit，这种配置`com.apple.developer.icloud`容器环境”的权利。可用选项：`Development` 和 `Production`。默认为Development
* `manifest`: - `Dictionary`  : 对于非App Store的出口，用户可以通过在Web浏览器中打开您的分发清单文件下载你的应用程序在网上。要生成分布明显，此键的值应该是有三个子键的字典：`appURL`，`displayImageURL`，`fullSizeImageURL`。额外的子键`assetPackManifestURL`是按需使用资源时，需要。
* `method`: - `String`  : Xcode中描述如何导出存档。可用选项：app-store, ad-hoc, package, enterprise, development, developer-id。选项列表会有所不同根据存档的类型。默认为 development
* `onDemandResourcesAssetPacksBaseURL`: - `String` 对于非App Store的出口，如果应用程序使用按需资源`embedOnDemandResourcesAssetPacksInBundle`不是YES，这应该是一个基本URL指定，其中资产包将要举行。该配置应用从指定的URL下载资产包
* `teamID`: - `String`  开发者门户网站团队使用这个出口。默认为球队用来建立档案
* `teamID`: - `String`  
* `uploadBitcode`: - `Bool` 对于App Store的导出，应包包括`bitcode`？默认为YES
* `uploadSymbols`: - `Bool` 对于App Store的出口，应包包含符号？默认为YES

# 示例

## 清除编译过程生成文件

```sh
xcodebuild clean install
```

## 编译项目

```sh
xcodebuild build 
-workspace <xxx.workspace> 
-scheme <schemeName> 
-configuration <Debug|Release> 
-sdk<sdkName>
```

## 编译并生成.xcarchive包

```sh
xcodebuild archive
-archivePath <archivePath> #生成的.xcarchive包存放路径
-workspace <XXX.xcworkspace>
-scheme <schemeNmae>
-configuration <Debug|Release>
-sdk <sdkName>
```

## 生成的.archive包导出成ipa文件

```sh
xcodebuild  -exportArchive
-archivePath <archivePath> #.archive文件的全路径 eg: .../.../XXX.xcarchive
-exportPath <exportPath> #ipa文件导出路径
-exportOptionsPlist <exportOptionsPlistPath> #exportOptionsPlist文件全路径 eg: .../.../XXX.plist
```

```sh
# Builds the targets Target1 and Target2 in the project MyProject.xcodeproj using the Debug configuration.
xcodebuild -project MyProject.xcodeproj -target Target1 -target Target2 -configuration Debug

# Builds the target MyTarget in the Xcode project in the directory from which xcodebuild was started, putting intermediate files in the directory /Build/MyProj/Obj.root and the products of the build in the directory /Build/MyProj/Sym.root.
xcodebuild -target MyTarget OBJROOT=/Build/MyProj/Obj.root SYMROOT=/Build/MyProj/Sym.root

# Builds the Xcode project in the directory from which xcodebuild was started against the macOS 10.6 SDK.  The canonical names of all available SDKs can be viewed using the -showsdks option.
xcodebuild -sdk macosx10.6

# Builds the scheme MyScheme in the Xcode workspace MyWorkspace.xcworkspace.
xcodebuild -workspace MyWorkspace.xcworkspace -scheme MyScheme

# Archives the scheme MyScheme in the Xcode workspace MyWorkspace.xcworkspace.
xcodebuild archive -workspace MyWorkspace.xcworkspace -scheme MyScheme

# Build tests and associated targets in the scheme MyScheme in the Xcode workspace MyWorkspace.xcworkspace using the generic iOS device destination. The command also writes test parameters from the scheme to an xctestrun file in the built products directory.
xcodebuild build-for-testing -workspace MyWorkspace.xcworkspace -scheme MyScheme -destination generic/platform=iOS

# Tests the scheme MyScheme in the Xcode workspace MyWorkspace.xcworkspace using both the iOS Simulator and the device named iPhone 5s for the latest version of iOS. The command assumes the test bundles are in the build root (SYMROOT).  (Note that the shell requires arguments to be quoted or otherwise escaped if they contain spaces.)
xcodebuild test-without-building -workspace MyWorkspace.xcworkspace -scheme MyScheme -destination 'platform=iOS Simulator,name=iPhone 5s' -destination 'platform=iOS,name=My iPad'

# Tests using both the iOS Simulator and the device named iPhone 5s.  Test bundle paths and other test parameters are specified in MyTestRun.xctestrun.  The command requires project binaries and does not require project source code.
xcodebuild test-without-building -xctestrun MyTestRun.xctestrun -destination 'platform=iOS Simulator,name=iPhone 5s' -destination 'platform=iOS,name=My iPad'

# Tests the scheme MyScheme in the Xcode workspace MyWorkspace.xcworkspace using the destination described as My Mac 64-bit in Xcode.
xcodebuild test -workspace MyWorkspace.xcworkspace -scheme MyScheme -destination 'platform=macOS,arch=x86_64'

# Tests the scheme MyScheme in the Xcode workspace MyWorkspace.xcworkspace using the destination described as My Mac 64-bit in Xcode. Only the test testFooWithBar of the test suite FooTests, part of the MyTests testing bundle target, will be run.
xcodebuild test -workspace MyWorkspace.xcworkspace -scheme MyScheme -destination 'platform=macOS,arch=x86_64' -only-testing MyTests/FooTests/testFooWithBar

# Exports the archive MyMobileApp.xcarchive to the path ExportDestination using the options specified in export.plist.
xcodebuild -exportArchive -archivePath MyMobileApp.xcarchive -exportPath ExportDestination -exportOptionsPlist 'export.plist'

# Exports two XLIFF files to MyDirectory from MyProject.xcodeproj containing development language strings and translations for Simplified Chinese and Mexican Spanish.
xcodebuild -exportLocalizations -project MyProject.xcodeproj -localizationPath MyDirectory -exportLanguage zh-hans -exportLanguage es-MX

# Export a single XLIFF file to MyDirectory from MyProject.xcodeproj containing only development language strings. (In this case, the -exportLanguage parameter has been excluded.)
xcodebuild -exportLocalizations -project MyProject.xcodeproj -localizationPath MyDirectory

# Imports localizations from MyLocalizations.xliff into MyProject.xcodeproj.  Translations with issues will be reported but not imported.
xcodebuild -importLocalizations -project MyProject.xcodeproj -localizationPath MyLocalizations.xliff

xcodebuild -version [-sdk [<sdkfullpath>|<sdkname>] [-json] [<infoitem>] ]

xcodebuild -list [[-project <projectname>]|[-workspace <workspacename>]] [-json]

xcodebuild -showsdks [-json]
xcodebuild -exportArchive -archivePath <xcarchivepath> [-exportPath <destinationpath>] -exportOptionsPlist <plistpath>
xcodebuild -exportNotarizedApp -archivePath <xcarchivepath> -exportPath <destinationpath>
xcodebuild -exportLocalizations -localizationPath <path> -project <projectname> [-exportLanguage <targetlanguage>...[-includeScreenshots]]
xcodebuild -importLocalizations -localizationPath <path> -project <projectname>
xcodebuild -resolvePackageDependencies [-project <projectname>|-workspace <workspacename>] -clonedSourcePackagesDirPath <path>
xcodebuild -create-xcframework [-help] [-framework <path>] [-library <path> [-headers <path>]] -output <path>

xcodebuild -exportArchive -archivePath xcarchivepath -exportPath destinationpath -exportOptionsPlist path
xcodebuild -exportNotarizedApp -archivePath xcarchivepath -exportPath destinationpath

xcodebuild -exportLocalizations -project name.xcodeproj -localizationPath path [[-exportLanguage language] ...]
xcodebuild -importLocalizations -project name.xcodeproj -localizationPath path
```

```sh
-project name.xcodeproj
-target targetname
-alltargets
-workspace name.xcworkspace
-scheme schemename

-destination destinationspecifier
-destination-timeout timeout

-configuration configurationname [Debug|Release]
-arch architecture #目标架构
-sdk [sdkfullpath | sdkname]
-showsdks

-showBuildSettings
-showdestinations
-showBuildTimingSummary
-showTestPlans
-list

-enableAddressSanitizer [YES | NO]
-enableThreadSanitizer [YES | NO]
-enableUndefinedBehaviorSanitizer [YES | NO]
-enableCodeCoverage [YES | NO]

-testLanguage language
-testRegion region

-derivedDataPath path # 建成功时相关的缓存文件路径，默认为 /Users/dasheng/Library/Developer/Xcode/DerivedData
-resultBundlePath path # 如果指定 path 那么APP相关日志（闪退、代码覆盖率报告、构建日志、屏幕快照....）都会保存在 path 下

-allowProvisioningUpdates
-allowProvisioningDeviceRegistration

-exportArchive
-exportNotarizedApp
-archivePath xcarchivepath
-exportPath destinationpath
-exportOptionsPlist path
-exportLocalizations
-importLocalizations
-localizationPath
-exportLanguage language
-xcconfig filename
-xctestrun xctestrunpath
-testPlan test-plan-name
-skip-testing test-identifier, -only-testing test-identifier
-skip-test-configuration test-configuration-name, -only-test-configuration test-configuration-name
-disable-concurrent-destination-testing
-maximum-concurrent-test-device-destinations number
-maximum-concurrent-test-simulator-destinations number
-parallel-testing-enabled [YES | NO]
-parallel-testing-worker-count number
-maximum-parallel-testing-workers number
-parallelize-tests-among-destinations
-test-timeouts-enabled [YES | NO]
-default-test-execution-time-allowance seconds
-maximum-test-execution-time-allowance seconds
-dry-run, -n
-skipUnavailableActions
buildsetting=value
-userdefault=value
-toolchain [identifier | name]
-quiet
-verbose
-version
-license
-checkFirstLaunchStatus
-runFirstLaunch
-usage
```

```sh
build # 构建target
build-for-testing # 构建target和对应的相关单元测试
analyze # 构建和分析target或scheme
archive # 
test # 
test-without-building # 
installsrc # 
install # 
clean # 
```

# 资料
* [Xcodebuild从入门到精通](https://www.hualong.me/2018/03/14/Xcodebuild/)


`xcodebuild -list -project SEGRongHui.xcodeproj`

```sh
Command line invocation:
    /Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild -list -project SEGRongHui.xcodeproj

Information about project "SEGRongHui":
    Targets:
        SEGRongHui

    Build Configurations:
        Debug
        Release

    If no build configuration is specified and -scheme is not passed then "Release" is used.

    Schemes:
        SEGRongHu
```

`xcodebuild -list -workspace SEGRongHui.xcworkspace `

```sh
Command line invocation:
    /Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild -list -workspace SEGRongHui.xcworkspace

Information about workspace "SEGRongHui":
    Schemes:
        AFNetworking
        AlipaySDK-iOS
        BaiduMapKit
        BMKLocationKit
        Commom
        CommonMediator
        commonPay
        commonThirds
        GDTMobSDK
        JCore
        JPush
        Pods-SEGRongHui
        ReactiveObjC
        SEGActivity
        SEGBill
        SEGCommunityBussiness
        SEGCommunitySocial
        SEGIntelligentHardware
        SEGMenberPoints
        SEGMenus
        SEGModel
        SEGOldModules
        SEGPlatform
        SEGPropertyBaseService
        SEGPropertyGrowthService
        SEGRongHui
        SEGStandard
        SEGViewModel
        SEGWebView
        SEGWebView-SEGWebView
        SEGWorkOrder
        SVProgressHUD
        Toast
        UMCCommon
        WechatOpenSDK
        YZAppSDK
```

`xcodebuild -showsdks [-json]`

```sh
iOS SDKs:
  iOS 14.1                        -sdk iphoneos14.1

iOS Simulator SDKs:
  Simulator - iOS 14.1            -sdk iphonesimulator14.1

macOS SDKs:
  DriverKit 19.0                  -sdk driverkit.macosx19.0
  macOS 10.15                     -sdk macosx10.15

tvOS SDKs:
  tvOS 14.0                       -sdk appletvos14.0

tvOS Simulator SDKs:
  Simulator - tvOS 14.0           -sdk appletvsimulator14.0

watchOS SDKs:
  watchOS 7.0                     -sdk watchos7.0

watchOS Simulator SDKs:
  Simulator - watchOS 7.0         -sdk watchsimulator7.0
```

`xcodebuild clean`

```sh
xcodebuild build \
-sdk iphoneos \
-workspace SEGRongHui.xcworkspace \
-scheme SEGRongHui \
-configuration Debug \
-allowProvisioningUpdates \
OBJROOT=$(PWD)/build \
SYMROOT=$(PWD)/build \
clean \
ONLY_ACTIVE_ARCH=YES TARGETED_DEVICE_FAMILY=1 
```

./builds

xcodebuild 
-archivePath "$ARCHIVE_PATH" 
-project "$(pwd)/repo/quantum_unity/Build/$PLATFORM/Unity-iPhone.xcodeproj" 
-sdk iphoneos 
-allowProvisioningUpdates 
-scheme 'Unity-iPhone' 
-configuration 'Release Development' 
archive DEVELOPMENT_TEAM=$TEAMID



xcodebuild -project SEGRongHui.xcodeproj -target SEGRongHui -target Pods  -configuration Debug

```sh
xcodebuild \
[-project name.xcodeproj] \
[[-target targetname] ... | -alltargets] \
[-configuration configurationname] \
[-sdk [sdkfullpath | sdkname]] \
[action ...] \
[buildsetting=value ...] \
[-userdefault=value ...]

xcodebuild \
[-project name.xcodeproj] \
-scheme schemename [[-destination destinationspecifier] ...] \
[-destination-timeout value] \
[-configuration configurationname] \
[-sdk [sdkfullpath | sdkname]] \
[action ...] \
[buildsetting=value ...] \
[-userdefault=value ...]

xcodebuild \
-workspace name.xcworkspace \
-scheme schemename [[-destination destinationspecifier] ...] \
[-destination-timeout value] \
[-configuration configurationname] \
[-sdk [sdkfullpath | sdkname]] \
[action ...] \
[buildsetting=value ...] \
[-userdefault=value ...]
```

SEGRongHui.xcodeproj
SEGRongHui.xcworkspace
SEGRongHui

xcodebuild -project SEGRongHui.xcodeproj -alltargets -configuration Debug build OBJROOT=$(PWD)/build SYMROOT=$(PWD)/build

```sh
xcodebuild clean build \
-sdk iphoneos \
-workspace SEGRongHui.xcworkspace \
-scheme SEGRongHui \
-configuration Debug \
-allowProvisioningUpdates \
OBJROOT=$(PWD)/build \
SYMROOT=$(PWD)/build \
-destination 'generic/platform=iOS' \
```

xcodebuild archive -archivePath $(PWD)/archives -workspace SEGRongHui.xcworkspace -scheme SEGRongHui -configuration Debug -sdk iphoneos

xcodebuild -exportArchive -archivePath archives.xcarchive -exportPath $(PWD)/ipas ExportDestination -exportOptionsPlist 'ExportOptions.plist'

xcodebuild  -exportArchive
-archivePath <archivePath> #.archive文件的全路径 eg: .../.../XXX.xcarchive
-exportPath <exportPath> #ipa文件导出路径
-exportOptionsPlist <exportOptionsPlistPath> #exportOptionsPlist文件全路径 eg: .../.../XXX.plist

xcodebuild -exportArchive -archivePath MyMobileApp.xcarchive -exportPath ExportDestination -exportOptionsPlist 'export.plist'  
xcodebuild -exportArchive -archivePath xcarchivepath -exportPath destinationpath -exportOptionsPlist path


-destination 'generic/platform=iOS'
-sdk iphoneos
-configuration Release

--configuration = Pods.xcconfig


```sh
xcodebuild build \
-sdk iphoneos \
-workspace SEGRongHui.xcworkspace \
-scheme SEGRongHui \
-configuration Release \
-allowProvisioningUpdates \
OBJROOT=$( PWD )/build \
SYMROOT=$( PWD )/build \
-destination 'generic/platform=iOS' \
-quiet 
```

* `-quiet` 忽略 warnings 打印日志。
* -arch arm64  -arch armv7

```sh
xcodebuild -exportArchive -archivePath archive文件的地址.xcarchive 
 -exportPath 导出的文件夹地址 \
 -exportOptionsPlist exprotOptionsPlist.plist \
 CODE_SIGN_IDENTITY=证书 \
 PROVISIONING_PROFILE=描述文件UUID  \
```

/Users/shanliu/Library/Developer/Xcode/DerivedData/SEGRongHui-atyahnzpxkckgoadplauiehyqlkh

```sh
xcrun -sdk iphoneos -v PackageApplication ./build/Release-iphoneos/$(target|scheme).app
```

```
CODE_SIGN_IDENTITY = "iPhone Distribution: companyname (9xxxxxxx9A)"
PROVISIONING_PROFILE = "xxxxx-xxxx-xxx-xxxx-xxxxxxxxx"
CONFIGURATION = "Release"
SDK = "iphoneos"

USER_KEY = "15d6xxxxxxxxxxxxxxxxxx" 是蒲公英开放 API 的密钥
API_KEY = "efxxxxxxxxxxxxxxxxxxxx" 是蒲公英开放 API 的密钥
```

CODE_SIGN_IDENTITY="iPhone Distribution: xxxxxxx"

* Xcode -> Preferences -> 选中申请开发者证书的 Apple ID -> 选中开发者证书 -> View Details… -> 根据 Provisioning Profiles 的名字选中打包所需的 mobileprovision 文件 -> 右键菜单 -> Show in Finder -> 找到该文件后，除了该文件后缀名的字符串就是 PROVISIONING_PROFILE 字段的内容。

* [Building from the Command Line with Xcode FAQ](https://developer.apple.com/library/archive/technotes/tn2339/_index.html)




Manual
No signing certificate "iOS Development" found

```
Missing private key for signing certificate.
Failed to locate the private key matching certificate "Apple Development: SHAN LAU (DP3HXX5B36)" in the keychain. To sign with this signing certificate, install its private key in your keychain. If you don't have the private key, select a different signing certificate for CODE_SIGN_IDENTITY in the build settings editor.
```

1. 钥匙串 -> `Apple Development: SHAN LAU (DP3HXX5B36)` -> 始终信任
2. `Repair Trust Settings`





























