# 函数宏

* `...`: 可变参数。
* `__VA_ARGS__`: 宏定义中的`...`中的所有剩余参数。
* `__FILE__`: 当前文件的绝对路径,常见于log中。
* `__LINE__`: 展开该宏时在文件中的行数,常见于log中。
* `__func__`: 也可以写成`__FUNCTION__`，所在scope的函数名称,常见于log中。
* `##` :连接符号。
* `#` :原样输出。
* `/` :换行符。
* `__DATE__`: 进行预处理的日期（”Mmm dd yyyy”形式的字符串）。
* `__TIME__`: 源文件编译时间（格式“hh:mm:ss”）。

# 官方宏

```objc
//表示下面定义的宏内容只被使用OC语言的文件所引用
#ifdef __OBJC__
#else
#endif
```

```objc
#ifdef DEBUG
#else
#endif
```