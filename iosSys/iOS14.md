# 相册

# 定位

## 位置敏感设置

在 iOS13 及以前，App 请求用户定位授权时为如下形态：一旦用户同意应用获取定位信息，当前应用就可以获取到用户的精确定位。<br>
iOS14 授权弹窗新增的 Precise的开关默认会选中精确位置。用户通过这个开关可以进行更改，当把这个值设为 On 时，地图上会显示精确位置；切换为Off时，将显示用户的大致位置。

在`Info.plist`中配置：

<img src="/assets/images/sys/01.png"/>

通过`CLLocationManager`中的方法用于向用户申请临时开启一次精确位置权限。

<img src="/assets/images/sys/02.png"/>

key 即为获取用户权限时传的 "purposeKey"，最终呈现给用户的就是左图，右图为当App主动关闭精确定位权限申请。

<img src="/assets/images/sys/03.png"/>

## 位置不敏感设置

在`Info.plist`中配置：

<img src="/assets/images/sys/04.png"/>

这样设置之后，即使用户想要为该 App 开启精确定位权限，也无法开启。

也可以直接通过API来根据不同的需求设置不同的定位精确度。需要注意的是，当 App 在 Background 模式下，如果并未获得精确位置授权，那么 Beacon 及其他位置敏感功能都将受到限制。

<img src="/assets/images/sys/05.png"/>

# Local Network

iOS14 当 App 要使用 Bonjour 服务时或者访问本地局域网，使用 mDNS 服务等，都需要授权，开发者需要在 Info.plist 中详细描述使用的为哪种服务以及用途。下图为需要无需申请权限与需要授权的服务：

<img src="/assets/images/sys/06.png"/>

用户可在 "隐私设置" 中也可以查看和修改具体有哪些 App 正在使用 `LocalNetwork`：

<img src="/assets/images/sys/07.png"/>

如果应用中需要使用 `LocalNetwork` 需要在 `Info.plist` 中配置两个选项，详细描述为什么需要使用该权限，以及需要列出具体使用 `LocalNetwork` 的服务列表。

<img src="/assets/images/sys/08.png"/>

对于使用了下列包含 `Bonjour` 的`framework`，都需要更新描述.

<img src="/assets/images/sys/09.png"/>

# Wi-Fi Address

`iOS8~iOS13`，用户在不同的网络间切换和接入时，`MAC`地址都不会改变，这也就使得网络运营商还是可以通过`MAC`地址对用户进行匹配和用户信息收集，生成完整的用户信息。<br>
`iOS14`提供`Wifi`加密服务，每次接入不同的`Wifi`使用的`MAC`地址都不同。每过 24 小时，`MAC`地址还会更新一次。需要关注是否有使用用户网络`MAC`地址的服务。

# 剪切板

在`iOS14`中，读取用户剪切板的数据会弹出提示。

<img src="/assets/images/sys/10.png"/>

<iframe width="560" height="315" src="https://www.youtube.com/embed/pRSWdtoUAjo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

弹出提示的原因是使用 `UIPasteboard` 访问用户数据，访问以下数据都会弹出 toast 提示。

<img src="/assets/images/sys/11.png"/>

下面两个 API 可用于判断剪切板中是否有`URL`，可用于规避提示。如果应用想直接访问剪切板的数据，暂时可能无法做到规避该提示。

<img src="/assets/images/sys/12.png"/>

下面两个 API 可以获得具体的 URL 信息，但是会触发剪切板提示。并且实测当用户剪切板中包含多个 URL 时只会返回第一个。

<img src="/assets/images/sys/13.png"/>

```objc
NSSet *patterns = [[NSSet alloc] initWithObjects:UIPasteboardDetectionPatternProbableWebURL, nil];
[[UIPasteboard generalPasteboard] detectPatternsForPatterns:patterns completionHandler:^(NSSet<UIPasteboardDetectionPattern> * _Nullable result, NSError * _Nullable error) {
    if (result && result.count) {
            // 当前剪切板中存在 URL
    }
}];
```

# 相机和麦克风

iOS14 中 App 使用相机和麦克风时会有图标提示以及绿点和黄点提示，并且会显示当前是哪个 App 在使用此功能。我们无法控制是否显示该提示。

会触发录音小黄点的代码示例：

```objc
AVAudioRecorder *recorder = [[AVAudioRecorder alloc] initWithURL:recorderPath settings:nil error:nil];
[recorder record];
```

触发相机小绿点的代码示例:

```objc
AVCaptureDeviceInput *videoInput = [[AVCaptureDeviceInput alloc] initWithDevice:videoCaptureDevice error:nil];
AVCaptureSession *session = [[AVCaptureSession alloc] init];
if ([session canAddInput:videoInput]) {
    [session addInput:videoInput];
}
[session startRunning];
```

# IDFA

IDFA 全称为 Identity for Advertisers ，即广告标识符。用来标记用户，目前最广泛的用途是用于投放广告、个性化推荐等。

在 iOS13 及以前，系统会默认为用户开启允许追踪设置，我们可以简单的通过代码来获取到用户的 IDFA 标识符。

```objc
if ([[ASIdentifierManager sharedManager] isAdvertisingTrackingEnabled]) {
    NSString *idfaString = [[ASIdentifierManager sharedManager] advertisingIdentifier].UUIDString;
    NSLog(@"%@", idfaString);
}
```

但是在 iOS14 中，这个判断用户是否允许被追踪的方法已经废弃。

<img src="/assets/images/sys/14.png"/>


iOS14 中，系统会默认为用户关闭广告追踪权限。

<img src="/assets/images/sys/15.png"/>

对于这种情况，我们需要去请求用户权限。首先需要在`Info.plist` 中配置`NSUserTrackingUsageDescription`及描述文案，接着使用 `AppTrackingTransparency` 框架中的 `ATTrackingManager` 中的 `requestTrackingAuthorizationWithCompletionHandler` 请求用户权限，在用户授权后再去访问 `IDFA` 才能够获取到正确信息。

```objc
#import <AppTrackingTransparency/AppTrackingTransparency.h>
#import <AdSupport/AdSupport.h>

- (void)testIDFA {
    if (@available(iOS 14, *)) {
        [ATTrackingManager requestTrackingAuthorizationWithCompletionHandler:^(ATTrackingManagerAuthorizationStatus status) {
            if (status == ATTrackingManagerAuthorizationStatusAuthorized) {
                NSString *idfaString = [[ASIdentifierManager sharedManager] advertisingIdentifier].UUIDString;
            }
        }];
    } else {
        // 使用原方式访问 IDFA
    }
}
```

对于用户拒绝授权 UserTracking 的情况，可以考虑接入苹果的[SKAdNetwork 框架](https://developer.apple.com/documentation/storekit/skadnetwork)进行广告分析。

# 上传 AppStore

更加严格的隐私审核，可以让用户在下载 App 之前就知道此 App 将会需要哪些权限。目前苹果商店要求所有应用在上架时都必须提供一份隐私政策。如果引入了第三方收集用户信息等SDK，都需要向苹果说明是这些信息的用途。

<img src="/assets/images/sys/16.png"/>

<img src="/assets/images/sys/17.png"/>

# 资料参考

* [【淘系技术】iOS14 隐私适配及部分解决方案](https://juejin.im/post/6850418120923250701)



