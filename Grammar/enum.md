
在iOS中用两个宏来描述枚举：`NS_ENUM`、`NS_OPTIONS`，`NS_ENUM`多用于一般枚举，`NS_OPTIONS`则多用于带有移位运算的枚举。

```objc
typedef NS_ENUM(NSInteger, UITableViewCellStyle) {
    UITableViewCellStyleDefault,
    UITableViewCellStyleValue1,	
    UITableViewCellStyleValue2,	
    UITableViewCellStyleSubtitle
};


typedef NS_OPTIONS(NSUInteger, Test) {
    TestA = 1 << 0,
    TestB = 1 << 1,
    TestC = 1 << 2,
    TestD = 1 << 3
};
```