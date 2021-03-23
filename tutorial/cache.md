# 系统沙盒系统API概述

```
├── Documents iTunes会同步此文件夹中的内容。
├── Library
│   ├── Caches 存放缓存文件,在应用退出时，该目录下的文件将被删除。
│   └── Preferences 存放偏好设置的文件(NSUserDefaults)，iTunes会同步此文件夹中的内容。
├── SystemData
└── tmp 在应用退出时，该目录下的文件将被删除;也可能在应用不运行时被删除。
```

```objc
// 要搜索的目录名称
typedef NS_ENUM(NSUInteger, NSSearchPathDirectory) {
    NSApplicationDirectory = 1,             // supported applications (Applications)
    NSDemoApplicationDirectory,             // unsupported applications, demonstration versions (Demos)
    NSDeveloperApplicationDirectory,        // developer applications (Developer/Applications). DEPRECATED - there is no one single Developer directory.
    NSAdminApplicationDirectory,            // system and network administration applications (Administration)
    NSLibraryDirectory,                     // various documentation, support, and configuration files, resources (Library)
    NSDeveloperDirectory,                   // developer resources (Developer) DEPRECATED - there is no one single Developer directory.
    NSUserDirectory,                        // user home directories (Users)
    NSDocumentationDirectory,               // documentation (Documentation)
    NSDocumentDirectory,                    // documents (Documents)
    NSCoreServiceDirectory,                 // location of CoreServices directory (System/Library/CoreServices)
    NSAutosavedInformationDirectory API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)) = 11,   // location of autosaved documents (Documents/Autosaved)
    NSDesktopDirectory = 12,                // location of user's desktop
    NSCachesDirectory = 13,                 // location of discardable cache files (Library/Caches)
    NSApplicationSupportDirectory = 14,     // location of application support files (plug-ins, etc) (Library/Application Support)
    
    NSDownloadsDirectory API_AVAILABLE(macos(10.5), ios(2.0), watchos(2.0), tvos(9.0)) = 15,              // location of the user's "Downloads" directory
    NSInputMethodsDirectory API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)) = 16,           // input methods (Library/Input Methods)
    NSMoviesDirectory API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)) = 17,                 // location of user's Movies directory (~/Movies)
    NSMusicDirectory API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)) = 18,                  // location of user's Music directory (~/Music)
    NSPicturesDirectory API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)) = 19,               // location of user's Pictures directory (~/Pictures)
    NSPrinterDescriptionDirectory API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)) = 20,     // location of system's PPDs directory (Library/Printers/PPDs)
    NSSharedPublicDirectory API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)) = 21,           // location of user's Public sharing directory (~/Public)
    NSPreferencePanesDirectory API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)) = 22,        // location of the PreferencePanes directory for use with System Preferences (Library/PreferencePanes)
    NSApplicationScriptsDirectory API_AVAILABLE(macos(10.8)) API_UNAVAILABLE(ios, watchos, tvos) = 23,      // 在iOS中不能使用
    NSItemReplacementDirectory API_AVAILABLE(macos(10.6), ios(4.0), watchos(2.0), tvos(9.0)) = 99,	    // For use with NSFileManager's URLForDirectory:inDomain:appropriateForURL:create:error:
    NSAllApplicationsDirectory = 100,       // all directories where applications can occur
    NSAllLibrariesDirectory = 101,          // all directories where resources can occur
    NSTrashDirectory API_AVAILABLE(macos(10.8), ios(11.0)) API_UNAVAILABLE(watchos, tvos) = 102             // location of Trash directory
};

// 搜索方位
typedef NS_OPTIONS(NSUInteger, NSSearchPathDomainMask) {
    NSUserDomainMask = 1,       // 当前应用APP的沙盒
    NSLocalDomainMask = 2,      // local to the current machine --- place to install items available to everyone on this machine (/Library)
    NSNetworkDomainMask = 4,    // publically available location in the local area network --- place to install items available on the network (/Network)
    NSSystemDomainMask = 8,     // provided by Apple, unmodifiable (/System)
    NSAllDomainsMask = 0x0ffff  // all domains: all of the above and future items
};

/*
NSSearchPathForDirectoriesInDomains(NSPreferencePanesDirectory, NSUserDomainMask, YES).firstObject;

NSAllApplicationsDirectory:      /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Applications
NSApplicationDirectory:          /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Applications
NSDemoApplicationDirectory:      /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Applications/Demos
NSAdminApplicationDirectory:     /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Applications/Utilities

NSDeveloperDirectory:            /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Developer
NSDeveloperApplicationDirectory: /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Developer/Applications

NSAllLibrariesDirectory:         /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Library
NSLibraryDirectory:              /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Library
NSDocumentationDirectory:        /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Library/Documentation
NSAutosavedInformationDirectory: /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Library/Autosave Information
NSCachesDirectory:               /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Library/Caches
NSApplicationSupportDirectory:   /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Library/Application Support
NSPreferencePanesDirectory:      /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Library/PreferencePanes
NSInputMethodsDirectory:         /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Library/Input Methods

NSDocumentDirectory:             /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Documents
NSDesktopDirectory:              /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Desktop
NSDownloadsDirectory:            /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Downloads
NSMoviesDirectory:               /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Movies
NSMusicDirectory:                /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Music
NSPicturesDirectory:             /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Pictures
NSSharedPublicDirectory:         /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/Public

NSTrashDirectory:(null)
NSUserDirectory:(null)
NSCoreServiceDirectory:(null)
NSPrinterDescriptionDirectory:(null)
NSItemReplacementDirectory:(null)
*/
```



```objc
NSLog(@"bundlePath: %@",[[NSBundle mainBundle] bundlePath]); // 获取应用目录
NSLog(@"NSHomeDirectory: %@",NSHomeDirectory()); // Home 目录
NSLog(@"NSTemporaryDirectory: %@",NSTemporaryDirectory()); // 应用沙盒 tmp 目录

/*
bundlePath:                      /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Bundle/Application/8441030F-D386-4251-A741-6309D73F8344/CacheExample_Example.app
NSHomeDirectory:                 /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE
NSTemporaryDirectory:            /Users/shanliu/Library/Developer/CoreSimulator/Devices/80A7D9BE-9BB0-4936-8C28-91324DB8BAEF/data/Containers/Data/Application/B56CE5A5-0050-4551-BC2B-91D5E2AA96DE/tmp/
*/
```

## 真机打开沙盒

<img src="/assets/images/tutorial/cache/05.png"/>

# 存储沙盒分析

## SDWebImage

```objc
- (IBAction)loadImageView:(id)sender {
    NSURL *url = [NSURL URLWithString:@"https://pgdt.ugdtimg.com/gdt/0/EAA6yOaAEsAEsAAAEthBfaESfAfkVMUue.jpg/0?ck=ab086a30bd9cf5df90b7562835a9c2ff"];
    [self.imageView sd_setImageWithURL:url];
    
    NSURL *urlR = [NSURL URLWithString:@"https://pgdt.ugdtimg.com/gdt/0/EABJa2MAUAALQAAAe63BgVF_vDoR-iuV9.jpg/0?ck=0711f826851a667daac9291ff39a57fb"];
    [self.rightImageView sd_setImageWithURL:urlR];
}
```

加载好图片看沙盒目录文件结构：

<img src="/assets/images/tutorial/cache/01.png"/>

可以看到`SDWebImage` 图片设置默认的目录位置`Library/Caches`，可以检测`com.hackemist.SDImageCache`目录下文件的总大小，从而进行内存的监测管理。

## 解档归档

```objc
- (IBAction)archive:(id)sender {
    NSString *path = [[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject] stringByAppendingPathComponent:@"a.txt"];
    [NSKeyedArchiver archiveRootObject:@"AAAA" toFile:path];
}
- (IBAction)unarchive:(id)sender {
    NSString *path = [[NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject] stringByAppendingPathComponent:@"a.txt"];
    [NSKeyedUnarchiver unarchiveObjectWithFile:path];
}
```

<img src="/assets/images/tutorial/cache/02.png"/>

## 偏好设置

```objc
- (IBAction)pre_get:(id)sender {
    [[NSUserDefaults standardUserDefaults] valueForKey:@"doorKey"];
}
- (IBAction)pre_set:(id)sender {
    [[NSUserDefaults standardUserDefaults] setValue:@"1" forKey:@"doorKey"];
}
```

<img src="/assets/images/tutorial/cache/03.png"/>

<img src="/assets/images/tutorial/cache/04.png"/>

# 文件操作

## 获取文件夹下所有文件集合

```objc
- (NSMutableArray*)getAllFilesInPath:(NSString *) path {
    NSMutableArray * arrRet = [NSMutableArray array];
    
    NSFileManager * fileManger = [NSFileManager defaultManager];
    BOOL isDir = NO;
    BOOL isExist = [fileManger fileExistsAtPath:path isDirectory:&isDir];
    if (isExist) {
        if (isDir) {
            NSError *error;
            NSArray * dirArray = [fileManger contentsOfDirectoryAtPath:path error:&error];
            if (!error) {
                NSString * subPath = nil;
                for (NSString * str in dirArray) {
                    subPath  = [path stringByAppendingPathComponent:str];
                    BOOL issubDir = NO;
                    [fileManger fileExistsAtPath:subPath isDirectory:&issubDir];
                    [arrRet addObjectsFromArray:[self getAllFilesInPath:subPath]];
                }
            }else {
                NSLog(@"%@:%@",error.domain,error.description);
            }
        }else{
            [arrRet addObject:path];
        }
    }
    
    return arrRet;
}
```

## 计算单文件大小

```objc
NSInteger sizeCount = 0;
for (NSString *p in pathes) {
    sizeCount += [[fileManager attributesOfItemAtPath:p error:nil][NSFileSize] integerValue];
}
```

## 文件尺寸大小转换

```objc
-(NSString*)fileSize:(NSUInteger)size {
    NSString *ret = @"0 M";
    float size_KB = size/1000;
    //ret = [NSString stringWithFormat:@"%d KB",(int)size_KB];
    float size_M = size_KB/1000;
    ret = [NSString stringWithFormat:@"%.3f M",size_M];
    return ret;
}
```

## 删除文件夹、文件

```objc
NSFileManager * fileManager = [NSFileManager defaultManager];
if ([fileManager isDeletableFileAtPath:path]) {
    NSError *error;
    [fileManager removeItemAtPath:path error:&error];
    if (error) NSLog(@"%@",error.description);
}
```











