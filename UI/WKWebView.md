

```
.
└── Library
    ├── Saved\ Application\ State
    │   └── com.sam.demo.WKWebViewExample.savedState
    │       └── KnownSceneSessions
    │           └── data.data
    └── SplashBoard
        └── Snapshots
            └── com.sam.demo.WKWebViewExample\ -\ {DEFAULT\ GROUP}
                └── 0C6792F8-C928-4635-901B-5359D4DD621A@3x.ktx
```

在执行`[WKWebsiteDataStore defaultDataStore]`时，APP会在沙盒创建，

```
├── Library
│   ├── Caches
│   │   └── com.sam.demo.WKWebViewExample
│   │       └── WebKit
│   │           ├── CacheStorage
│   │           ├── NetworkCache
│   │           ├── OfflineWebApplicationCache
│   │           └── ServiceWorkers
│   ├── Saved\ Application\ State
│   │   └── com.sam.demo.WKWebViewExample.savedState
│   │       └── KnownSceneSessions
│   │           └── data.data
│   ├── SplashBoard
│   │   └── Snapshots
│   │       └── com.sam.demo.WKWebViewExample\ -\ {DEFAULT\ GROUP}
│   │           └── 0C6792F8-C928-4635-901B-5359D4DD621A@3x.ktx
│   └── WebKit
│       └── com.sam.demo.WKWebViewExample
│           └── WebsiteData
│               ├── IndexedDB
│               ├── LocalStorage
│               ├── MediaKeys
│               ├── ResourceLoadStatistics
│               └── WebSQL
└── tmp
    └── com.sam.demo.WKWebViewExample
        └── WebKit
            └── MediaCache
```

当APP有Cookies新增时:

# NSURLCache

```
├── Library
│   ├── Caches
│   │   └── com.sam.demo.WKWebViewExample
│   │       ├── Cache.db [NSURLCache 新增]
│   │       ├── Cache.db-shm [NSURLCache 新增]
│   │       ├── Cache.db-wal [NSURLCache 新增]
│   │       └── WebKit
│   │           ├── CacheStorage
│   │           ├── NetworkCache
│   │           ├── OfflineWebApplicationCache
│   │           └── ServiceWorkers
│   ├── Saved\ Application\ State
│   │   └── com.sam.demo.WKWebViewExample.savedState
│   │       └── KnownSceneSessions
│   │           └── data.data
│   ├── SplashBoard
│   │   └── Snapshots
│   │       └── com.sam.demo.WKWebViewExample\ -\ {DEFAULT\ GROUP}
│   │           └── 0C6792F8-C928-4635-901B-5359D4DD621A@3x.ktx
│   └── WebKit
│       └── com.sam.demo.WKWebViewExample
│           └── WebsiteData
│               ├── IndexedDB
│               ├── LocalStorage
│               ├── MediaKeys
│               ├── ResourceLoadStatistics
│               └── WebSQL
└── tmp
    └── com.sam.demo.WKWebViewExample
        └── WebKit
            └── MediaCache
```

当执行`[NSHTTPCookieStorage sharedHTTPCookieStorage]` 和`[WKWebsiteDataStore defaultDataStore]` 方法时，沙盒文件没有别的变动。现在APP本地新增了`Cache.db`数据哭，可以遍历`NSHTTPCookieStorage`和`WKWebsiteDataStore`观察各自容器的 cookies 情况。

```objc
- (IBAction)printHttpCookies:(id)sender {
    NSHTTPCookieStorage *cookieJar = [NSHTTPCookieStorage sharedHTTPCookieStorage];
    NSArray *cookies = [NSArray arrayWithArray:[cookieJar cookies]];
    for (NSHTTPCookie *cookie in cookies) {
        NSLog(@"HTTP: %@:%@",cookie.name,cookie.value);
    }
}
- (IBAction)printWKCookies:(id)sender {
    [WKWebsiteDataStore defaultDataStore];
    [[WKWebsiteDataStore defaultDataStore].httpCookieStore getAllCookies:^(NSArray<NSHTTPCookie *> * _Nonnull cookies) {
        for (NSHTTPCookie *cookie in cookies) {
            NSLog(@"WK: %@:%@",cookie.name,cookie.value);
        }
    }];
}

// HTTP: JSESSIONID:14F5261C1DE3917143F6953F8BCB4A4C
// WK: JSESSIONID:14F5261C1DE3917143F6953F8BCB4A4C
```

从打印结果看，系统内部对`NSHTTPCookieStorage`和`WKWebsiteDataStore`的cookeis进行了同步。

一种情况是，在APP本地沙盒存在`Cache.db`的情况下，杀掉APP进程再次进入APP，查看`NSHTTPCookieStorage`和`WKWebsiteDataStore`的cookies携带情况，结果两个打印出来的都是空，是否说明在未再次请求网络的情况下，就算本地存在`Cache.db`数据库，`NSHTTPCookieStorage`和`WKWebsiteDataStore`也不会获取 db 里面的 cookies值。


查看`Cache.db`内部情况：

```sh
% sqlite3 Cache.db
SQLite version 3.28.0 2019-04-15 14:49:49
Enter ".help" for usage hints.
sqlite> .table
cfurl_cache_blob_data      cfurl_cache_response     
cfurl_cache_receiver_data

# 可以看到 Cache.db 数据中包含3哥数据表
```

```sql
-- select * from sqlite_master where type="table" and name="cfurl_cache_blob_data";
CREATE TABLE cfurl_cache_blob_data(
	entry_ID INTEGER PRIMARY KEY, 
	response_object BLOB, 
	request_object BLOB, 			  
	proto_props BLOB, 
	user_info BLOB
)

-- select * from cfurl_cache_blob_data;
-- 1|bplist00?WVersionUArray??__CFURLStringType\_CFURLString_=https://beta.uhomecp.com/uhomecp-app/advertising/advlist.json#A???kw | bplist00?WVersionUArray??__CFURLStringType\_CFURLString_=https://beta.uhomecp.com/uhomecp-app/advertising/advlist.json#@N | | 

-- select * from sqlite_master where type="table" and name="cfurl_cache_response";
CREATE TABLE cfurl_cache_response(
	entry_ID INTEGER PRIMARY KEY AUTOINCREMENT UNIQUE,
	version INTEGER, 
	hash_value INTEGER, 
	storage_policy INTEGER, 
	request_key TEXT UNIQUE, 	 
	time_stamp NOT NULL DEFAULT CURRENT_TIMESTAMP, 
	partition TEXT
)

-- select * from cfurl_cache_response;
-- 1|0|7366274791171153819|0|https://beta.uhomecp.com/uhomecp-app/advertising/advlist.json|2021-01-12 09:01:10|

-- select * from sqlite_master where type="table" and name="cfurl_cache_receiver_data";
CREATE TABLE cfurl_cache_receiver_data(
	entry_ID INTEGER PRIMARY KEY, 
	isDataOnFS INTEGER, 
	receiver_data BLOB
)
-- select * from cfurl_cache_receiver_data;
-- 1|0|{"code" : "0000002", "message" : "未登录或已过期， 请重新登录！"}
```

`NSHTTPCookie` 中包含很多关于Cookies的信息：

```objc
<NSHTTPCookie
	version:0
	name:snsuid
	value:124721028923392
	expiresDate:'2022-01-08 13:35:03 +0000'
	created:'2021-01-08 05:35:05 +0000'
	sessionOnly:FALSE
	domain:.gdt.qq.com
	partition:none
	path:/
	isSecure:FALSE
	isHTTPOnly: YES
 path:"/" isSecure:FALSE isHTTPOnly: YES>
```

# NSHTTPCookieStorage

```objc
// 清除 cookeis
[[NSHTTPCookieStorage sharedHTTPCookieStorage] removeCookiesSinceDate:[NSDate dateWithTimeIntervalSince1970:0]];
```

# WKWebsiteDataStore

```objc
// 清除 cookies
if (@available(iOS 9.0, *)) {
    [[WKWebsiteDataStore defaultDataStore] removeDataOfTypes:[WKWebsiteDataStore allWebsiteDataTypes] modifiedSince:[NSDate dateWithTimeIntervalSince1970:0] completionHandler:^{}];
}
```

`WKProcessPool` has its own cycle cookies.

# websites

* [Setting cookies with WKHTTPCookieStorage do not sync with wkwebview](https://developer.apple.com/forums/thread/97194)
