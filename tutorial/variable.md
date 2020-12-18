
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

## staic、const、extern

* `const`: 
* `extern`: 
* `static`: 表示的是静态的。
	* 修饰局部变量: 
	* 修饰全局变量: 

* 生命周期 这个变量能存活多久，它所占用的内存什么时候分配，什么时候收回。
* 作用域

```objc
static NSString * const SDAlternateImageOperationKey = @"NSButtonAlternateImageOperation";
```