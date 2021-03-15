* [iOS静态库开发中引入的第三方库可能与宿主APP中冲突的解决方案](https://blog.csdn.net/a1661408343/article/details/105661766)

* ar: create and maintain library archives
* Mach-O: Mach-O assembler and link editor output
* stab: symbol table types
* nlist: retrieve symbol table name list from an executable file
* dyldinfo: Displays information used by dyld in an executable

```
-a     Display all symbol table entries, including those inserted for use by debuggers.
-g     Display only global (external) symbols. 只展示所有非私有(external)的符号。
-n     Sort numerically rather than alphabetically.
-o     Prepend file or archive element name to each output line, rather than only once.
-p     Don't sort; display in symbol-table order.
-r     Sort in reverse order.
-u     Display only undefined symbols.
-U     Don't display undefined symbols.
-m     Display  the  N_SECT type symbols (Mach-O symbols) as (segment_name, section_name) followed by either external or non-external and then the symbol name.  Undefined, com-
       mon, absolute and indirect symbols get displayed as (undefined), (common), (absolute), and (indirect), respectively. Other symbol  details  are  displayed  in  a  human-
       friendly  manner,  such as "[no dead strip]".  nm will display the referenced symbol for indirect symbols and will display the name of the library expected to provide an
       undefined symbol. See nlist(3) and <mach-o/nlist.h> for more information on the nlist structure.
-x     Display the symbol table entry's fields in hexadecimal, along with the name as a string.
-j     Just display the symbol names (no value or type).
-s segname sectname
       List only those symbols in the section (segname,sectname).  For llvm-nm(1) this option must be last on the command line, and after the files.
-l     List a pseudo symbol .section_start if no symbol has as its value the starting address of the section.  (This is used with the -s option above.)
-arch arch_type
       Specifies the architecture, arch_type, of the file for nm(1) to operate on when the file is a universal file (see arch(3)  for  the  currently  known  arch_types).   The
       arch_type can be "all" to operate on all architectures in the file.  The default is to display the symbols from only the host architecture, if the file contains it; oth-
       erwise, symbols for all architectures in the file are displayed.
-f  format
       For llvm-nm(1) this specifies the output format.  Where format can be bsd, sysv, posix or darwin.
-f     For nm-classic(1) this displays the symbol table of a dynamic library flat (as one file not separate modules).  This is obsolete and not supported with llvm-nm(1).
-A     Write the pathname or library name of an object on each line.
-P     Write information in a portable output format.
-t format
       For the -P output, write the numeric value in the specified format. The format shall be dependent on the single character used as the format option-argument:
d      The value shall be written in decimal (default).
o      The value shall be written in octal.
x      The value shall be written in hexadecimal.
-L     Display the symbols in the bitcode files in the (__LLVM,__bundle) section if present instead of the object's symbol table. For nm-classic(1) this is the default  if  the
       object  has  no  symbol  table  and  an (__LLVM,__bundle) section exists. This option is not supported by llvm-nm(1) where displaying llvm bitcode symbols is the default
       behavior.
```

* `U` 该符号在当前文件中是未定义的，即该符号的定义在别的文件中。

```
// nm命令输出
U _NSDefaultRunLoopMode

// 在 AFURLRequestSerialization 类中对应的代码：
[inputStream scheduleInRunLoop:[NSRunLoop currentRunLoop] forMode:NSDefaultRunLoopMode];
```

`NSDefaultRunLoopMode` 在 `AFURLRequestSerialization` 中没有定义，是在别的文件中定义的。

* `S` 没有别的表示符号。
* `T` 该符号位于代码区text section。

* `_OBJC_CLASS_$_` 类名。




```sh
nm -gUm AFNetworking
```

产生的结果：

```sh
# 获取二进制中实现的global符号
% nm -gUm AFNetworking 
0000000000020130 (__TEXT,__text) external _AFJSONObjectByRemovingKeysWithNullValues
00000000000407f0 (__DATA,__const) external _AFNetworkingOperationFailingURLRequestErrorKey
0000000000040938 (__DATA,__const) external _AFNetworkingOperationFailingURLResponseDataErrorKey
0000000000040930 (__DATA,__const) external _AFNetworkingOperationFailingURLResponseErrorKey
0000000000040748 (__DATA,__const) external _AFNetworkingReachabilityDidChangeNotification
0000000000040750 (__DATA,__const) external _AFNetworkingReachabilityNotificationStatusItem
00000000000409b0 (__DATA,__const) external _AFNetworkingTaskDidCompleteAssetPathKey
00000000000409a8 (__DATA,__const) external _AFNetworkingTaskDidCompleteErrorKey
0000000000040968 (__DATA,__const) external _AFNetworkingTaskDidCompleteNotification
00000000000409a0 (__DATA,__const) external _AFNetworkingTaskDidCompleteResponseDataKey
0000000000040998 (__DATA,__const) external _AFNetworkingTaskDidCompleteResponseSerializerKey
0000000000040990 (__DATA,__const) external _AFNetworkingTaskDidCompleteSerializedResponseKey
00000000000409b8 (__DATA,__const) external _AFNetworkingTaskDidCompleteSessionTaskMetrics
0000000000040960 (__DATA,__const) external _AFNetworkingTaskDidResumeNotification
0000000000040970 (__DATA,__const) external _AFNetworkingTaskDidSuspendNotification
0000000000035bb0 (__TEXT,__const) external _AFNetworkingVersionNumber
0000000000035b80 (__TEXT,__const) external _AFNetworkingVersionString
00000000000137d4 (__TEXT,__text) external _AFPercentEscapedStringFromString
0000000000013f80 (__TEXT,__text) external _AFQueryStringFromParameters
00000000000141e0 (__TEXT,__text) external _AFQueryStringPairsFromDictionary
0000000000014250 (__TEXT,__text) external _AFQueryStringPairsFromKeyAndValue
0000000000010390 (__TEXT,__text) external _AFStringFromNetworkReachabilityStatus
00000000000407e8 (__DATA,__const) external _AFURLRequestSerializationErrorDomain
0000000000040928 (__DATA,__const) external _AFURLResponseSerializationErrorDomain
0000000000040978 (__DATA,__const) external _AFURLSessionDidInvalidateNotification
0000000000040988 (__DATA,__const) external _AFURLSessionDownloadTaskDidFailToMoveFileNotification
0000000000040980 (__DATA,__const) external _AFURLSessionDownloadTaskDidMoveFileSuccessfullyNotification
000000000004c828 (__DATA,__objc_data) external _OBJC_CLASS_$_AFActivityIndicatorViewNotificationObserver
000000000004c008 (__DATA,__objc_data) external _OBJC_CLASS_$_AFAutoPurgingImageCache
000000000004bfe0 (__DATA,__objc_data) external _OBJC_CLASS_$_AFCachedImage
000000000004c6e8 (__DATA,__objc_data) external _OBJC_CLASS_$_AFCompoundResponseSerializer
000000000004c418 (__DATA,__objc_data) external _OBJC_CLASS_$_AFHTTPBodyPart
000000000004c378 (__DATA,__objc_data) external _OBJC_CLASS_$_AFHTTPRequestSerializer
000000000004c558 (__DATA,__objc_data) external _OBJC_CLASS_$_AFHTTPResponseSerializer
000000000004c058 (__DATA,__objc_data) external _OBJC_CLASS_$_AFHTTPSessionManager
000000000004c170 (__DATA,__objc_data) external _OBJC_CLASS_$_AFImageDownloadReceipt
000000000004c198 (__DATA,__objc_data) external _OBJC_CLASS_$_AFImageDownloader
000000000004c120 (__DATA,__objc_data) external _OBJC_CLASS_$_AFImageDownloaderMergedTask
000000000004c0d0 (__DATA,__objc_data) external _OBJC_CLASS_$_AFImageDownloaderResponseHandler
000000000004c698 (__DATA,__objc_data) external _OBJC_CLASS_$_AFImageResponseSerializer
000000000004c4b8 (__DATA,__objc_data) external _OBJC_CLASS_$_AFJSONRequestSerializer
000000000004c5a8 (__DATA,__objc_data) external _OBJC_CLASS_$_AFJSONResponseSerializer
000000000004c3f0 (__DATA,__objc_data) external _OBJC_CLASS_$_AFMultipartBodyStream
000000000004c1e8 (__DATA,__objc_data) external _OBJC_CLASS_$_AFNetworkActivityIndicatorManager
000000000004c288 (__DATA,__objc_data) external _OBJC_CLASS_$_AFNetworkReachabilityManager
000000000004c508 (__DATA,__objc_data) external _OBJC_CLASS_$_AFPropertyListRequestSerializer
000000000004c648 (__DATA,__objc_data) external _OBJC_CLASS_$_AFPropertyListResponseSerializer
000000000004c328 (__DATA,__objc_data) external _OBJC_CLASS_$_AFQueryStringPair
000000000004c878 (__DATA,__objc_data) external _OBJC_CLASS_$_AFRefreshControlNotificationObserver
000000000004c2d8 (__DATA,__objc_data) external _OBJC_CLASS_$_AFSecurityPolicy
000000000004c3a0 (__DATA,__objc_data) external _OBJC_CLASS_$_AFStreamingMultipartFormData
000000000004c7d8 (__DATA,__objc_data) external _OBJC_CLASS_$_AFURLSessionManager
000000000004c738 (__DATA,__objc_data) external _OBJC_CLASS_$_AFURLSessionManagerTaskDelegate
000000000004c5f8 (__DATA,__objc_data) external _OBJC_CLASS_$_AFXMLParserResponseSerializer
000000000004c260 (__DATA,__objc_data) external _OBJC_CLASS_$_PodsDummy_AFNetworking
000000000004c7b0 (__DATA,__objc_data) external _OBJC_CLASS_$__AFURLSessionTaskSwizzling
000000000004c850 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFActivityIndicatorViewNotificationObserver
000000000004c030 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFAutoPurgingImageCache
000000000004bfb8 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFCachedImage
000000000004c710 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFCompoundResponseSerializer
000000000004c490 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFHTTPBodyPart
000000000004c3c8 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFHTTPRequestSerializer
000000000004c580 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFHTTPResponseSerializer
000000000004c080 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFHTTPSessionManager
000000000004c148 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFImageDownloadReceipt
000000000004c1c0 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFImageDownloader
000000000004c0f8 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFImageDownloaderMergedTask
000000000004c0a8 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFImageDownloaderResponseHandler
000000000004c6c0 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFImageResponseSerializer
000000000004c4e0 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFJSONRequestSerializer
000000000004c5d0 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFJSONResponseSerializer
000000000004c468 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFMultipartBodyStream
000000000004c210 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFNetworkActivityIndicatorManager
000000000004c2b0 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFNetworkReachabilityManager
000000000004c530 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFPropertyListRequestSerializer
000000000004c670 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFPropertyListResponseSerializer
000000000004c350 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFQueryStringPair
000000000004c8a0 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFRefreshControlNotificationObserver
000000000004c300 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFSecurityPolicy
000000000004c440 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFStreamingMultipartFormData
000000000004c800 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFURLSessionManager
000000000004c760 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFURLSessionManagerTaskDelegate
000000000004c620 (__DATA,__objc_data) external _OBJC_METACLASS_$_AFXMLParserResponseSerializer
000000000004c238 (__DATA,__objc_data) external _OBJC_METACLASS_$_PodsDummy_AFNetworking
000000000004c788 (__DATA,__objc_data) external _OBJC_METACLASS_$__AFURLSessionTaskSwizzling
0000000000035bd0 (__TEXT,__const) external _kAFUploadStream3GSuggestedDelay
0000000000035bc8 (__TEXT,__const) external _kAFUploadStream3GSuggestedPacketSize
```