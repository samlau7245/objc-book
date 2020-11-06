# 计算机内存

> **1 Btye = 8 Bit**，这是一种规约，也出现过1Byte=7/8/9Bit这种，不过都是小众情况，具体是根据ASCII码来确定的，因为ASCII码是`8Bit`存储(7Bit+1Bit的校验格式)。

<img src="/assets/images/tutorial/12.png"/>

```c
int main(int argc, const char * argv[]) {
    printf("long 存储大小 : %lu 字节\n", sizeof(unsigned long)); // long 存储大小 : 8 字节
    
    printf("CHAR 最小值: %d\n", CHAR_MIN );// -128(2^7)+ 2^1 的符号位
    printf("CHAR 最大值: %d\n", CHAR_MAX );// 127(2^7-1)+ 2^1 的符号位
    
    printf("INT8 最小值: %d\n", INT8_MIN );// -128(2^7) + 2^1 的符号位
    printf("INT8 最大值: %d\n", INT8_MAX );// 127(2^7-1) + 2^1 的符号位
    
    printf("INT16 最小值: %d\n", INT16_MIN );// -32768(2^15) + 2^1 的符号位
    printf("INT16 最大值: %d\n", INT16_MAX );// 32767(2^15-1) + 2^1 的符号位
    
    printf("LONG 最小值: %ld\n", LONG_MIN );// -9223372036854775808
    printf("LONG 最大值: %ld\n", LONG_MAX );// 9223372036854775807

    return 0;
}
```

# NSObject类的底层数据结构

* 将OC转C++: `clang -rewrite-objc main.m -o main.cpp`
* 将OC转C++(精确支持平台、架构): `xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main.cpp`

通过OC转C++后的代码中有一段`NSObject`的实现代码,这基本就是`NSObject`的底层实现：

```c++
struct NSObject_IMPL {
    Class isa;
};
```

查看`NSObject`系统实现：

```objc
@interface NSObject <NSObject> {
    Class isa; // isa既然是个指针，那么在64位架构中占用内存为:8字节(Byte)，那么在32位架构中占用内存为:4字节(Byte)
}
```

而`Class`的的含义： **Class是OC类作用时指向底层结构体的指针。**

```c++
/// An opaque type that represents an Objective-C class.
typedef struct objc_class *Class; // 指针
```

NSObject底层实现结构图：

<img src="/assets/images/tutorial/13.png"/>

```objc
int main(int argc, const char * argv[]) {
    /*
     [[NSObject alloc] init]
     底层创建了一个8字节(Byte)的内存空间，然后把isa指针存放到这个内存空间中
     */
    NSObject *obj = [[NSObject alloc] init]; //
    NSLog(@"%p",obj); // 0x10053dc00 <- 这就是isa在内存空间中的地址 ps: 等价于 `struct NSObject_IMPL { Class isa;};`结构体在内存空间的地址==>（isa地址 == 结构体地址）
    return 0;
}
```


# 类与对象的内存大小

先看一段代码，思考一个问题：`一个`NSObject`对象占用多少内存？`

```objc
int main(int argc, const char * argv[]) {
    NSObject *obj = [[NSObject alloc] init];
    NSLog(@"%zu 字节",class_getInstanceSize(NSObject.class)); // 8 字节
    NSLog(@"%zd 字节",malloc_size((__bridge const void *)(obj))); // 16 字节
    return 0;
}
```

* 先看下`class_getInstanceSize`的底层实现。

<img src="/assets/images/tutorial/14.png"/>

> `class_getInstanceSize`方法就是获取实例对象中成员变量内存大小。

* 看下`[NSObject alloc]` 分配内存空间 底层做了什么：

<img src="/assets/images/tutorial/15.png"/>

> `CF requires all objects be at least 16 bytes.`， 最终是要求创建所有的对象大小必须 `>= 16 byte`。

<img src="/assets/images/tutorial/16.png"/>

* 扩展一下，现在有一个Person对象:

```objc
@interface Person : NSObject
@end
@implementation Person
@end
```

<img src="/assets/images/tutorial/17.png"/>

>* `typedef long NSInteger;` ，NSInteger底层是一个`long`类型，按照C规则是`8Byte`。
>* `name` 算是对象的成员变量，`8Byte`。

* 看一下两个自定义对象的内存：

```objc
@interface Student : NSObject
@property(nonatomic,assign) NSInteger sex;
@property(nonatomic,strong) NSString *number;
@end
@implementation Student
@end

@interface Person : NSObject
@property(nonatomic,strong) Student *student;
@property(nonatomic,strong) NSString *name;
@end
@implementation Person
@end

// ++++++ ++++++ ++++++ ++++++ ++++++ ++++++ ++++++
int main(int argc, const char * argv[]) {
    NSObject *obj = [[Person alloc] init];
    NSLog(@"%zu 字节",class_getInstanceSize(Person.class)); // 24 字节
    NSLog(@"%zd 字节",malloc_size((__bridge const void *)(obj))); // 32 字节
    return 0;
}
int main(int argc, const char * argv[]) {
    NSObject *obj = [[Student alloc] init];
    NSLog(@"%zu 字节",class_getInstanceSize(Student.class)); // 24 字节
    NSLog(@"%zd 字节",malloc_size((__bridge const void *)(obj))); // 32 字节
    return 0;
}
```

<img src="/assets/images/tutorial/18.png"/>

* [苹果官方开源地址](https://opensource.apple.com/tarballs/)
下载[苹果官方开源地址-objc4](https://opensource.apple.com/tarballs/objc4/)可以找到`class_getInstanceSize`方法的实现：












































