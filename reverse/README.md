
# 越狱环境搭建

* 支持`RM64`、`iOS8`
* [Cydia AppStore ( Jailbreak All iOS Versions )](https://www.baidu.com/link?url=lSV6SkMuIqs_uE7C13L_nzfblxl4rlsvlauEh2FtdUe&wd=&eqid=b6a2733800118226000000065f98dad7)

## 判断是否越狱

```objc
// 判断设备是否越狱
//1. 判断是否安装Cydia
if ([[NSFileManager defaultManager] fileExistsAtPath:@"/Applications/Cydia.app"]) {
    NSLog(@"设备已经越狱！");
}else {
    NSLog(@"设备未越狱！");
}
```

## Mac搭建逆向工具

* `Alfred` 便捷搜索
* `XtraFinder` 增强型Finder
* `iTerm2` 完爆Terminal的工具
* `Go2Shell` 从Finder中快速定位到命令行的工具

## Mac远程登录到iPhone

iOS、macOS都是基于`Unix`操作系统的，都可以通过命令行对这些设备进行操作。

```
ssh -- OpenSSH SSH client (remote login program)
```

* `OpenSSH` 是SSH协议的免费开源实现，可以通过 OpenSSH 方式让Mac远程登录到iPhone。

# Cycript

* `Cycript` : 是Objective-C++、ES6(JavaScript)、Java等语法的混合物。
* 可以用来探索、修改、调试正在运行的Mac、iOS APP。
* 官网:[http://www.cycript.org/](http://www.cycript.org/)
* 通过Cydia安装Cycript，即可在iPhone上调试运行中的APP。

# 混淆

## 加固

* 加固是为了增加应用的安全性，防止应用被破解、盗版、二次打包、注入、反编译等等。

常见的加固方式：

* 数据加密(字符串、网络数据、敏感数据等等)。
* 应用加壳(二进制加密，默认Apple是对上传的IPA进行了加壳，只不过被破解了)。
* 代码混淆(类名、方法名、代码逻辑等等进行混淆)。

## 代码混淆

* iOS可以通过`class-dump`、`hooper`、`IDA`等来获取类名、方法名、以及分析程序的执行逻辑。

* [底层下-原理-138-Runloop](https://www.bilibili.com/video/BV1ae411s7qo?p=343)
* [底层下-原理-235-架构设计](https://www.bilibili.com/video/BV1ae411s7qo?p=440)
* [底层下-原理-226-性能优化](https://www.bilibili.com/video/BV1ae411s7qo?p=431)
* [底层下-原理-193-内存管理](https://www.bilibili.com/video/BV1ae411s7qo?p=398)
* [底层下-原理-160-多线程](https://www.bilibili.com/video/BV1ae411s7qo?p=365)
* [底层下-原理-089-Runtime](https://www.bilibili.com/video/BV1ae411s7qo?p=294)
* [底层下-原理-064-block](https://www.bilibili.com/video/BV1ae411s7qo?p=269)




























































