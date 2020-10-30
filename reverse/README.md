
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