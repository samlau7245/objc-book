
# 常量

```objc
//.h
FOUNDATION_EXPORT NSString * const AFNetworkingTaskDidResumeNotification;
//.m
NSString * const AFNetworkingTaskDidResumeNotification = @"com.alamofire.networking.task.resume";
```

* 全局静态常量

```objc
static NSString * const AFURLSessionManagerLockName = @"com.alamofire.networking.session.manager.lock";
```