
[1601-Category的本质01-基本使用](https://www.bilibili.com/video/BV1ae411s7qo?p=181)

# Category分类本质

<img src="/assets/images/tutorial/05.png "/>

* 不会为分类创建新的对象。
* 分类方法`-test`和对象方法`-run`都是放在类(class)中。
* 分类方法`+test1`和对象方法`+run1`都是放在原类(meta-class)中。

```objc
@interface Person : NSObject
-(void)run;
+(void)run1;
@end
 
@interface Person (Test)
-(void)test;
+(void)test1;
@end
```

# 内存分配

