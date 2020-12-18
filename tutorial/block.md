# 基本使用

## 定义

### block 作为类型

* `.h`定义文件：

```objc
#import <Foundation/Foundation.h>

typedef void(^TestBlock)(NSInteger arg1,NSString * _Nullable arg2);
typedef NSInteger(^TestReturnBlock)(NSInteger arg1,NSString * _Nullable arg2);

@interface Manager : NSObject
-(void)funcA:(NSString*)argA block:(TestBlock)block;
-(void)funcB:(NSString*)argB block:(TestReturnBlock)block;
@end
```

* `.m`实现文件：

```objc
#import "Manager.h"

@implementation Manager

-(void)funcA:(NSString*)argA block:(TestBlock)block {
    block(1,argA);
}

-(void)funcB:(NSString*)argB block:(TestReturnBlock)block {
    NSInteger ret = block(2,argB);
    NSLog(@"%s -- %zd",__FUNCTION__,ret); // -[Manager funcB:block:] -- 3
}
@end
```

* 调用：

```objc
int main(int argc, const char * argv[]) {
    
    Manager *m = [[Manager alloc] init];
    
    [m funcA:@"A" block:^(NSInteger arg1, NSString * _Nullable arg2) {
        NSLog(@"%s -- %zd -- %@",__FUNCTION__,arg1,arg2); // main_block_invoke -- 1 -- A
    }];
    
    [m funcB:@"B" block:^NSInteger(NSInteger arg1, NSString * _Nullable arg2) {
    	NSLog(@"%s -- %zd -- %@",__FUNCTION__,arg1,arg2); // main_block_invoke_2 -- 2 -- B
        return 3;
    }];

    return 0;
}
```