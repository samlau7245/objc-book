
# 给分类(category)添加属性(property)

`.h`文件

```objc
@interface NSButton (WebCache)
// 新增分类属性
@property (nonatomic, strong, readonly, nullable) NSURL *sd_currentImageURL;
@end
```

`.m`文件

```objc
#import "objc/runtime.h"

@implementation NSButton (WebCache)
// Getter
- (NSURL *)sd_currentImageURL {
    return objc_getAssociatedObject(self, @selector(sd_currentImageURL));
}
// Setter
- (void)setSd_currentImageURL:(NSURL *)sd_currentImageURL {
    objc_setAssociatedObject(self, @selector(sd_currentImageURL), sd_currentImageURL, OBJC_ASSOCIATION_RETAIN_NONATOMIC);
}
@end
```