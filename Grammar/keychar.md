# 关键字

## const

* `const`: 被修饰的变量为只读，不能修改。

```objc
//声明一个int类型的变量a，变量初始化值为10，并且变量a左边有一个const关键字修饰
int  const  a = 10;
//因为变量a被const修饰，就成为了只读，不能被修改赋值了，所以下面这行代码是错误的
a = 20;//错误代码
//上面第一句代码和这句代码是等价的，都是修饰变量a让其只读
const  int   a = 10;
int  const  *p   //  *p只读 ;p变量
int  * const  p  // *p变量 ; p只读
const  int   * const p //p和*p都只读
int  const  * const  p   //p和*p都只读
```

> 判断p 和p是只读还是变量，关键是看const在谁前面。如果只在p前面，那么p只读，p还是变量；如果在p前面,那么p只读 ，p变量。

## static

### 修饰局部变量

`static`修饰局部变量，保证变量只会被初始化一次，在内存中只有一份。

```objc
-(void)touchesBegan:(NSSet<UITouch *> *)touches withEvent:(UIEvent *)event {
  static  int i = 0;
    i ++;
    NSLog(@"i=%d",i);
}
// 2016-10-26 15:07:34.276 fff[2817:175155] i=1
// 2016-10-26 15:07:35.347 fff[2817:175155] i=2
// 2016-10-26 15:07:35.761 fff[2817:175155] i=3
// 2016-10-26 15:07:36.057 fff[2817:175155] i=4
// 2016-10-26 15:07:36.415 fff[2817:175155] i=5
```

### 修饰全局变量

`static`修饰的全局变量只能在当前文件内使用。

```objc
@implementation LoginTool

//static修饰全局变量，让外界文件无法访问
static LoginTool *_sharedManager = nil;

+ (LoginTool *)sharedManager {
     static dispatch_once_t oncePredicate;
    dispatch_once(&oncePredicate, ^{
        _sharedManager = [[self alloc] init];
    });
    return _sharedManager;
}
```

### 修饰函数

被`static`修饰的函数称为`静态函数`，外部无法访问，只有内部可以访问`静态函数`。


## extern

`extern`用来声明外部全局变量；`extern`只能用于声明不能实现。

基本用法：

```objc
/***********.h*************/
//声明一些全局常量
extern NSString * const name;
extern NSInteger const count;

/***********.m*************/
//实现
NSString * const name = @"王五";
NSInteger const count = 3;
```

# 忽略警告语法

```objc
#pragma clang diagnostic push  
#pragma clang diagnostic ignored "-相关命令"  
 // code here
#pragma clang diagnostic pop
```

* `-Wdeprecated-declarations`: 方法弃用警告。
* `-Wincompatible-pointer-types`: 不兼容指针类型。
* `-Warc-retain-cycles`: 循环引用警告。
* `-Wunused-variable`: 未使用变量警告。
* `-Wdeprecated-declarations`: 已废弃属性或方法警告。

# Debug、Release

```objc
#ifdef DEBUG
// debug code here
#else
// release code here
#endif
```





















