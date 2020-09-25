
# 通过Aspect来实现内容拦截

项目需要添加依赖:

```ruby
pod 'Aspects'
```

## 代码实现

创建拦截器

```objc
@interface ViewControllerInterceptor : NSObject
@end

#import "ViewControllerInterceptor.h"
#import "UIViewController+Addition.h"

static ViewControllerInterceptor *_instance;

@implementation ViewControllerInterceptor

// 会在应用启动的时候自动被runtime调用，通过这个方法可以实现代码的注入
+(void)load {
    [super load];
    [ViewControllerInterceptor sharedInstance];
}

/// 单例
+(instancetype)sharedInstance {
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
        _instance = [[self alloc] init];
    });
    return _instance;
}

// 拦截的方法在 init 中实现。
- (instancetype)init {
    self = [super init];
    if (self) {
        [UIViewController aspect_hookSelector:@selector(viewWillAppear:) withOptions:AspectPositionAfter usingBlock:^(id aspectInfo,BOOL animated){
            UIViewController *vc = [aspectInfo instance];
            if (!vc.disabledInterceptor) { // 针对某些 ViewController 不进行拦截
                [self viewWillAppear:animated viewController:vc];
            }
        } error:NULL];
    }
    return self;
}

// 通过这种方式可以代替原来框架中的基类，不必每个 ViewController 再去继续原框架的基类
#pragma mark - fake methods
- (void)viewWillAppear:(BOOL)animated viewController:(UIViewController *)viewController
{
    // 去做基础业务相关的内容
    if (!viewController.isInitTheme) {
        [self ThemeDidNeedUpdateStyle];
        viewController.isInitTheme = YES;
    }
    // 其他操作......
}

- (void)ThemeDidNeedUpdateStyle {
    NSLog(@"Theme did need update style");
}
@end
```

创建分类变量

```objc
@interface UIViewController (Addition)
@property(nonatomic, assign) BOOL isInitTheme;
@property(nonatomic, assign) BOOL disabledInterceptor;// 拦截器是否有效
@end


#define KeyIsInitTheme @"KeyIsInitTheme"
#define KeyDisabledInterceptor @"KeyDisabledInterceptor"

#import "UIViewController+Addition.h"
#import <objc/runtime.h>


@implementation UIViewController (Addition)
#pragma mark - inline property

- (BOOL)isInitTheme {
    return objc_getAssociatedObject(self, KeyIsInitTheme);
}

- (void)setIsInitTheme:(BOOL)isInitTheme {
    objc_setAssociatedObject(self, KeyIsInitTheme, @(isInitTheme), OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}

-(BOOL)disabledInterceptor {
    return objc_getAssociatedObject(self, KeyDisabledInterceptor);
}

-(void)setDisabledInterceptor:(BOOL)disabledInterceptor {
    objc_setAssociatedObject(self, KeyDisabledInterceptor, @(disabledInterceptor), OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}
@end
```

* [AutoNet-ios 拦截器实现](https://blog.csdn.net/weixin_34008933/article/details/91366765)
* [拦截器-网络状态监控](https://www.jianshu.com/p/fa5a18723174)