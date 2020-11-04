* [http://llvm.org/](http://llvm.org/)
* `LLVM`项目是模块化和可重用的编译器及工具链技术的集合。
* [Bilibili - llvm](https://search.bilibili.com/all?keyword=llvm&from_source=nav_search_new)

# 传统编译器架构

* 传统编译器架构种类：`GCC`、`LVVM`、`Clang`。

<img src="/assets/images/tutorial/06.png"/>

* FrontEnd: 会对SourceCode进行语法分析、词法分析、语义分析，生成中间代码。
* Optimizer: 对中间代码进行优化。
* BackEnd: 生成机器码。
* 不管FrontEnd使用的是什么语言，只要遵循LVVM架构，生成出来的中间代码都是`IR`格式。

iOS中的Clang与LVVM：

<img src="/assets/images/tutorial/07.png"/>

我们是如何自定义一个`Optimizer`放到编译架构中：

<img src="/assets/images/tutorial/08.png"/>

## OC文件的编译过程

* 查看编译过程： `clang -ccc-print-phases main.m`

```
% clang -ccc-print-phases main.m
               +- 0: input, "main.m", objective-c
            +- 1: preprocessor, {0}, objective-c-cpp-output # preprocessor 预处理器
         +- 2: compiler, {1}, ir # 优化器
      +- 3: backend, {2}, assembler #编译器后端
   +- 4: assembler, {3}, object # 目标代码
+- 5: linker, {4}, image # 链接(静态库、动态库)
6: bind-arch, "x86_64", {5}, image
```

* 查看编译过程 `clang -E main.m`

## 词法分析

* `clang -fmodules -E -Xclang -dump-tokens main.m`

<img src="/assets/images/tutorial/09.png"/>

<img src="/assets/images/tutorial/11.png"/>

## 语法树(AST)

* 语法树(AST-Abstract Syntax Tree)。
* `clang -fmodules -fsyntax-only -Xclang -ast-dump main.m`

<img src="/assets/images/tutorial/10.png"/>

## IR


























