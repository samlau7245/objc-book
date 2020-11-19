
xcodebuild 编译Xcode的`projects`和`workspaces`。

* 在一个`project`中，`xcodebuild`可以编译一个或者多个`targets`。
* 在一个`workspace`中，`xcodebuild`可以编译一个`project`。
* 在一个`workspace`中，`xcodebuild`可以编译一个`scheme`。
* 要编译一个`project`,在包含`name.xcodeproj`的目录下运行`xcodebuild`，如果当前目录存在多个`.xcodeproj`文件可以使用`[-project name.xcodeproj]`来指定需要编译的`project`。
* 在`project`中如果有多个`targets`，`xcodebuild`默认情况下会编译第一个`target`。
* 要编译一个`workspace`,需要同时指定`-workspace`、`-scheme`这两个参数，`-scheme`会控制哪一个`targets`被编译和如何编译。

**projects**

```
xcodebuild [-project name.xcodeproj] 
		   
		   [[-target targetname] ... | -alltargets] 

		   [-configuration configurationname]
           [-sdk [sdkfullpath | sdkname]] 
           [action ...] 
           [buildsetting=value ...] 
           [-userdefault=value ...]

xcodebuild [-project name.xcodeproj] 
		   
		   -scheme schemename 
		   [[-destination destinationspecifier] ...]
		   [-destination-timeout value]

           [-configuration configurationname] 
           [-sdk [sdkfullpath | sdkname]] 
           [action ...] 
           [buildsetting=value ...]
           [-userdefault=value ...]           
```

**workspace**

```
xcodebuild -workspace name.xcworkspace
		   
		   -scheme schemename 
		   [[-destination destinationspecifier] ...] 
		   [-destination-timeout value]
           
           [-configuration configurationname] 
           [-sdk [sdkfullpath | sdkname]] 
           [action ...] 
           [buildsetting=value ...]
           [-userdefault=value ...]
```

**查看**

```
xcodebuild -version 
		   [-sdk [sdkfullpath | sdkname]] 
		   [infoitem]

xcodebuild -showsdks

xcodebuild -showBuildSettings 
		   [-project name.xcodeproj | [-workspace name.xcworkspace -scheme schemename]]

xcodebuild -showdestinations 
		   [-project name.xcodeproj | [-workspace name.xcworkspace -scheme schemename]]

xcodebuild -showTestPlans 
           [-project name.xcodeproj | -workspace name.xcworkspace] 
           -scheme schemename

xcodebuild -list 
		   [-project name.xcodeproj | -workspace name.xcworkspace]           
```

**导出**

```
xcodebuild -exportArchive 
           -archivePath xcarchivepath 
           -exportPath destinationpath 
           -exportOptionsPlist path

xcodebuild -exportNotarizedApp 
           -archivePath xcarchivepath 
           -exportPath destinationpath

xcodebuild -exportLocalizations 
           -project name.xcodeproj 
           -localizationPath path 
           [[-exportLanguage language] ...]
```

**导入**

```
xcodebuild -importLocalizations 
		   -project name.xcodeproj 
		   -localizationPath path
```

--- 

**Options**

* `-project name.xcodeproj`
* `-target targetname`
* `-alltargets`
* `-workspace name.xcworkspace`
* `-scheme schemename`
* `-destination destinationspecifier` => 通过`destinationspecifier`来指定目标设备，默认是scheme选中的设备。
* `-destination-timeout timeout` => 搜索目标设备的超时时间。(默认：30秒)
* `-configuration configurationname` => build configuration
* `-arch architecture` => architecture
* `-sdk [sdkfullpath | sdkname]`
* `-showsdks`
* `-showBuildSettings` => 显示 build settings 配置。
	* 依赖参数: [-project | -workspace -scheme]
* `-showdestinations`
	* 依赖参数: [-project | -workspace -scheme]
* `-showBuildTimingSummary` => build时显示报告。
* `-showTestPlans`
	* 依赖参数: -scheme
* `-list` => 显示 project 中的所有 targets 和 configurations
	* 依赖参数: [-project | -workspace]
* `-enableAddressSanitizer [YES | NO]` 
	* 使用: scheme 设置
* `-enableThreadSanitizer [YES | NO]`
	* 使用: scheme 设置
* `-enableUndefinedBehaviorSanitizer [YES | NO]`
	* 使用: scheme 设置
* `-enableCodeCoverage [YES | NO]` => 代码覆盖率
	* 使用: test action
* `-testLanguage language` => 在测试的时候指定`ISO 639-1`语言。
	* 使用: test action
* `-testRegion region` => 在测试的时候指定`ISO 639-1`区域。
	* 使用: test action
* `-derivedDataPath path` => 在执行 action 时导出数据的路径。
* `-resultBundlePath path` => 在执行 action 时创建一个 bundle 并且导出到path路径；路径会被自动创建，如果path存在则会报错。
	* bundle中包含：日志、代码覆盖率报告、XML-测试结果、截图和在测试中的其它一些附件。
* `-allowProvisioningUpdates` => 允许 xcodebuild 和 Apple Developer 官方进行通讯，让 xcodebuild 可以自动创建、更新(profile、 app IDs、certificates)，为了达到这一目的， xcodebuild 需要下载或更新 provisioning profiles。
	* 必须： 有一个开发者帐号在 Xcode's Accounts preference 中。
* `-allowProvisioningDeviceRegistration` => 允许 xcodebuild 注册目标设备到 Apple Developer 官方。
	* 依赖参数：`-allowProvisioningUpdates`
* `-exportArchive`
	* 依赖参数：`-archivePath`、`-exportOptionsPlist`、`exportPath`
* `-exportNotarizedApp`
	* 依赖参数：`-archivePath`、`exportPath`
* `-archivePath xcarchivepath` => 指定一个 path 在执行 `archive action`时。
* `-exportPath destinationpath` => 指定一个到处产品的目标路径，包括需要导出产品的文件名。
* `-exportOptionsPlist path` => 这个是在 `-exportArchive` 中被用到。
* `-exportLocalizations` => 到处本地化到 XLIFF 文件。
	* 依赖参数：`-project`、`-localizationPath`
* `-importLocalizations` => 从 XLIFF 文件导入 本地化。
	* 依赖参数：`-project`、`-localizationPath`
* `-localizationPath` => XLIFF 文件 文件路径。
* `-exportLanguage language` => 导出
* `-xcconfig filename`
* `-xctestrun xctestrunpath` => 只能用在 `test-without-building action`
* `-testPlan test-plan-name`
* `-skip-testing test-identifier, -only-testing test-identifier`
* `-skip-test-configuration test-configuration-name, -only-test-configuration test-configuration-name`
* `-disable-concurrent-destination-testing`
* `-maximum-concurrent-test-device-destinations number` => 最大的测试目标设备的数量。
* `-maximum-concurrent-test-simulator-destinations number` => 最大的测试目标模拟器的数量。
* `-parallel-testing-enabled [YES | NO]`
* `-parallel-testing-worker-count number`
* `-maximum-parallel-testing-workers number`
* `-parallelize-tests-among-destinations`
* `-test-timeouts-enabled [YES | NO]` => 启动、关闭 测试超时行为。
* `-default-test-execution-time-allowance seconds`
* `-maximum-test-execution-time-allowance seconds`
* `-dry-run, -n` => 打印应该被执行但是没有被执行的命令
* `-skipUnavailableActions` => 跳过无效的 action。
* `-buildsetting=value` => 设置一个 buildsetting的值。 [Build settings reference](https://help.apple.com/xcode/mac/current/#/itcaec37c2a6)
* `-userdefault=value`
* `-toolchain [identifier | name]`
* `-quiet` => 不要打印任何 warnings、errors
* `-verbose`
* `-version` => 显示安装的Xcode的版本号。
* `-license`
* `-checkFirstLaunchStatus` => 第一次启动时检查状态
* `-runFirstLaunch` => 安装包并且同意条款政策。
* `-usage`


**action** : 指派一个或者多个 actions 来执行

| action | 描述 | 依赖 | 
| --- | --- | --- |
| `build` |(默认action)| -scheme |
| `build-for-testing` ||-scheme|
| `analyze` ||-scheme|
| `archive` ||-scheme|
| `test` ||-scheme、[-destination]|
| `test-without-building` ||[-scheme|-xctestrun]|
| `installsrc` |||
| `install` |||
| `clean` |||

示例：

```
xcodebuild archive -workspace MyWorkspace.xcworkspace -scheme MyScheme
xcodebuild build-for-testing -workspace MyWorkspace.xcworkspace -scheme MyScheme -destination generic/platform=iOS
```

--- 

**Destinations** 指定设备的目标平台

* `macOS`
* `iOS` => 需要提供 `id(设备ID)`、`name(设备名称)`
* `iOS Simulator`
* `watchOS`
* `watchOS Simulator`
* `tvOS`
* `tvOS Simulator`

**退出编译Code**


* `EX_OK` => 编译成功。
* `EX_USAGE` => 参数错误。
* `EX_NOINPUT` => 未找到导入的文件。
* `EX_IOERR` => 文件不能被读或者写。
* `EX_SOFTWARE` => xcodebuild fail。


**示例**

```sh
# Cleans the build directory; then builds and installs the first target in the Xcode project in the directory from which xcodebuild was started.
xcodebuild clean install

# Builds the targets Target1 and Target2 in the project MyProject.xcodeproj using the Debug configuration.
xcodebuild -project MyProject.xcodeproj -target Target1 -target Target2 -configuration Debug

# putting intermediate files in the directory /Build/MyProj/Obj.root and the products of the build in the directory /Build/MyProj/Sym.root.
xcodebuild -target MyTarget OBJROOT=/Build/MyProj/Obj.root SYMROOT=/Build/MyProj/Sym.root

xcodebuild -sdk macosx10.6

xcodebuild -workspace MyWorkspace.xcworkspace -scheme MyScheme

xcodebuild archive -workspace MyWorkspace.xcworkspace -scheme MyScheme

xcodebuild build-for-testing -workspace MyWorkspace.xcworkspace -scheme MyScheme -destination generic/platform=iOS
xcodebuild test-without-building -workspace MyWorkspace.xcworkspace -scheme MyScheme -destination 'platform=iOS Simulator,name=iPhone 5s' -destination 'platform=iOS,name=My iPad'

xcodebuild -exportArchive -archivePath MyMobileApp.xcarchive -exportPath ExportDestination -exportOptionsPlist 'export.plist'

xcodebuild -exportLocalizations -project MyProject.xcodeproj -localizationPath MyDirectory -exportLanguage zh-hans -exportLanguage es-MX

xcodebuild -exportLocalizations -project MyProject.xcodeproj -localizationPath MyDirectory

xcodebuild -importLocalizations -project MyProject.xcodeproj -localizationPath MyLocalizations.xliff
```


```sh
~ FastlaneExample_01 % xcodebuild -showsdks
iOS SDKs:
	iOS 14.1                      	-sdk iphoneos14.1

iOS Simulator SDKs:
	Simulator - iOS 14.1          	-sdk iphonesimulator14.1

macOS SDKs:
	DriverKit 19.0                	-sdk driverkit.macosx19.0
	macOS 10.15                   	-sdk macosx10.15

tvOS SDKs:
	tvOS 14.0                     	-sdk appletvos14.0

tvOS Simulator SDKs:
	Simulator - tvOS 14.0         	-sdk appletvsimulator14.0

watchOS SDKs:
	watchOS 7.0                   	-sdk watchos7.0

watchOS Simulator SDKs:
	Simulator - watchOS 7.0       	-sdk watchsimulator7.0

~ FastlaneExample_01 % xcodebuild -sdk macosx10.15
```

**报错**

```
error: Signing for "FastlaneExample_01" requires a development team. Select a development team in the Signing & Capabilities editor. (in target 'FastlaneExample_01' from project 'FastlaneExample_01')
```

FastlaneExample_01
xcodebuild archive -workspace FastlaneExample_01.xcworkspace -scheme FastlaneExample_01
xcodebuild build-for-testing -workspace FastlaneExample_01.xcworkspace -scheme FastlaneExample_01 -destination generic/platform=iOS

xcodebuild -workspace FastlaneExample_01.xcworkspace -scheme FastlaneExample_01 -PRODUCT_BUNDLE_IDENTIFIER=com.hdwy.uhome

PRODUCT_BUNDLE_IDENTIFIER
-buildsetting

xcodebuild -workspace FastlaneExample_01.xcworkspace -scheme FastlaneExample_01 -configuration Debug PRODUCT_BUNDLE_IDENTIFIER=com.hdwy.uhome
xcodebuild -target FastlaneExample_01 -configuration Debug PRODUCT_BUNDLE_IDENTIFIER=com.hdwy.uhome

xcodebuild -project "FastlaneExample_01.xcodeproj" -scheme "FastlaneExample_01" -sdk "iphoneos" -configuration Release PROVISIONING_PROFILE="MyProject_ProvisioningProfile" DEVELOPEMENT_TEAM="MY_TEAM_ID"
xcodebuild -project "FastlaneExample_01.xcodeproj" -scheme "FastlaneExample_01" -sdk "iphoneos" -configuration Release DEVELOPEMENT_TEAM="6MP44W2MKJ"


* `$(BUILT_PRODUCTS_DIR)`
* `$(TARGET_NAME)`
* `$(SRCROOT)`
* `$(CURRENT_PROJECT_VERSION)`
* `$(SYMROOT)`
* `$(BUILD_DIR)`
* `$(BUILD_ROOT)`




> @see also: `ibtool(1), sysexits(3), xcode-select(1), xcrun(1), xed(1)`




xcodebuild -project "FastlaneExample_01.xcodeproj" -scheme "FastlaneExample_01" -allowProvisioningUpdates -allowProvisioningDeviceRegistration DEVELOPEMENT_TEAM="6MP44W2MKJ"
xcodebuild -project "FastlaneExample_01.xcodeproj" -scheme "FastlaneExample_01" -allowProvisioningUpdates -allowProvisioningDeviceRegistration PRODUCT_BUNDLE_IDENTIFIER=com.fun.more

-project FastlaneExample_01.xcodeproj
-workspace FastlaneExample_01.xcworkspace
-scheme FastlaneExample_01
-allowProvisioningUpdates -allowProvisioningDeviceRegistration
-configuration Debug

PRODUCT_BUNDLE_IDENTIFIER=com.fun.more
DEVELOPEMENT_TEAM=U3NQKAD5EF

xcodebuild -workspace FastlaneExample_01.xcworkspace -scheme FastlaneExample_01 -configuration Debug DEVELOPEMENT_TEAM=U3NQKAD5EF PRODUCT_BUNDLE_IDENTIFIER=com.fun.more  -allowProvisioningUpdates -allowProvisioningDeviceRegistration

Team Name SHAN LAU
Team ID U3NQKAD5EF
---

```sh
xcodebuild -version
# Xcode 12.1
# Build version 12A7403
```

certificate
provisioning profile 




* [Xcode Release Notes](https://developer.apple.com/library/archive/releasenotes/DeveloperTools/RN-Xcode/Chapters/Introduction.html#//apple_ref/doc/uid/TP40001051-CH1-SW136)
* [XCODE PROVISIONING PROFILE AUTOMATION FOR CI](https://www.testdevlab.com/blog/2019/07/xcode-provisioning-profile-automation-for-ci/)
* [iOS 自动构建命令——xcodebuild](https://www.jianshu.com/p/3f43370437d2)


To make an iOS project build automatically on a remote machine using manual signing configuration, we need to follow certain steps:

* Uncheck Automatically manage signing checkbox in the Project settings,
* Provide specific Provisioning profiles in Project settings,
* Copy a valid Apple Development and/or Production Certificate to the remote build machine,
* Download and copy all chosen Provisioning Profiles from the 2nd step to the remote build machine.

Signing 变成手动管理 => Uncheck Automaticlly manage signing

```
CODE_SIGN_STYLE = Automatic # 自动管理
CODE_SIGN_STYLE = Manual # 手动管理
```

CODE_SIGN_RESOURCE_RULES_PATH 
 /usr/bin/codesign


**签名**

* 手动签名
	* 需要手动指定`Code Signing`、`Provisioning Profile`。
* 自动签名 
	* Code Signing Style : Automatic
	* Code Signing Identity : iOS Developer
	* Provisioning Profile : Automatic
	* PROVISIONING_PROFILE 清空

# Archive APP

```sh
# @see: https://help.apple.com/xcode/mac/current/#/dev86f8cf93d
xcodebuild -exportArchive -archivePath [your.xcarchive] -exportOptionsPlist ExportOptions.plist -allowProvisioningUpdates -allowProvisioningDeviceRegistration
```

# Create an XCFramework

```sh
# @see:https://help.apple.com/xcode/mac/current/#/dev544efab96
xcodebuild archive  
	[-project <project name>] 
	-scheme <scheme name> 
	-destination "generic/platform=<platform name>[,arch=<architecture name>][,variant=<variant name>]" 
	[-configuration <configuration name>] 
	[-archivePath <archive output path>]

xcodebuild -create-xcframework -framework <path> [-framework <path>...] -output <path>

xcodebuild -create-xcframework -library <path> [-headers <path>] [-library <path> [-headers <path>]...] -output <path>
```

# 测试

* [Xcode Help - Manage tests](https://help.apple.com/xcode/mac/current/#/devc38fc7392)