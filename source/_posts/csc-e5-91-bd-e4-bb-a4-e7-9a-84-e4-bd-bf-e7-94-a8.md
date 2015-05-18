title: "CSC命令的使用"
date: 2013-12-04 09:53:42
tags:
id: 422
comment: false
categories:
  - 学习笔记
---

csc是C#的命令行编译器。

我们这群习惯用命令行的人，要去建立什么VS项目确实挺困难的，不过和谐的社会总是从最简单的命令行开始的，所以命令行提供了想象不到的方便与快捷。

**csc.exe编译器常用命令：**

1.  命令：**csc File.cs** ，功能：编译 File.cs以产生 File.exe，另一种写法：csc/out:MyFile.exe File.cs，可以自定义编译生成的执行文件名称
2.  命令：**csc/target:library File.cs** ，功能：编译 File.cs 以产生 File.dll，另一种写法：csc /target:library /out:MyFile.dll  File.cs，可以自定义编译生成的库文件名称
3.  命令：**csc/define:DEBUG /optimize /out:File2.exe *.cs** ，功能：通过使用优化和定义 DEBUG 符号，编译当前目录中所有的 C# 文件，输出为 File2.exe
4.  命令：**csc /target:library /out:File2.dll /warn:0 /nologo /debug *.cs** ，功能：编译当前目录中所有的 C# 文件，以产生File2.dll 的调试版本，不显示任何徽标和警告
5.  命令：**csc /target:library /out:Something.xyz *.cs** ，功能：将当前目录中所有的 C# 文件编译为Something.xyz（一个DLL）
**csc.exe编译器命令详解：**

编译器位置：C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727\csc.exe

- 编译器选项 -                     -   输出文件   -
/out:&lt;文件&gt;                       输出文件名（默认值：包含主类的文件或第一个文件的基名称）
/target:exe                       生成控制台可执行文件（默认）  (缩写:   /t:exe)
/target:winexe                    生成Windows可执行文件         (缩写:   /t:winexe)
/target:library                   生成库                        (缩写:   /t:library)
/target:module                    生成能添加到其他程序集的模块  (缩写:   /t:module)
/define:&lt;符号列表&gt;                定义条件编译符号              (缩写:   /d)

/doc:&lt;文件&gt;                       要生成的XML文档文件
/recurse:&lt;通配符&gt;                 根据通配符规范，包括当前目录和子目录下的所有文件
/reference:&lt;文件列表&gt;             从指定的程序集文件引用元数据  (缩写:   /r)
/addmodule:&lt;文件列表&gt;             将指定的模块链接到此程序集中

-   资源文件    -
/win32res:&lt;文件&gt;                  指定Win32资源文件(.res)
/win32icon:&lt;文件&gt;                 使用该图标输出
/resource:&lt;资源信息&gt;              嵌入指定的资源                (缩写:   /res)
/linkresource:&lt;资源信息&gt;          将指定的资源链接到此程序集中  (缩写:   /linkres)

-   代码调试    -
/debug[+|-]                       发出调试信息
/debug:{full|pdbonly}             指定调试类型（“full”是默认类型，可以将调试程序附加到正在运行的程序）
/optimize[+|-]                    启用优化                      (缩写:   /o)
/incremental[+|-]                 启用增量编译                  (缩写:   /incr)

-   错误和警告   -
/warnaserror[+|-]                 将警告视为错误
/warn:&lt;n&gt;                         设置警告等级(0-4)             (缩写:   /w)
/nowarn:&lt;警告列表&gt;                禁用特定的警告消息

-   语言   -
/checked[+|-]                     生成溢出检查
/unsafe[+|-]                      允许“不安全”代码

-   杂项   -
@&lt;文件&gt;                           读取响应文件以获得更多选项
/help                             显示此用法信息                (缩写:   /?)
/nologo                           取消编译器版权信息
/noconfig                         不要自动包含CSC.RSP文件

-   高级   -
/baseaddress:&lt;地址&gt;               要生成的库的基址
/bugreport:&lt;文件&gt;                 创建一个“错误报告”文件
/codepage:&lt;n&gt;                     指定打开源文件时要使用的代码页
/utf8output                       UTF-8编码的输出编译器消息
/main:&lt;类型&gt;                      指定包含入口点的类型（忽略所有其他可能的入口点）   (缩写:   /m)
/fullpaths                        编译器生成完全限定路径
/filealign:&lt;n&gt;                    指定用于输出文件节的对齐方式
/nostdlib[+|-]                    不引用标准库(mscorlib.dll)
/lib:&lt;文件列表&gt;                   指定要在其中搜索引用的附加目录

引用dll实例

csc /reference:zjdx.dll test.cs