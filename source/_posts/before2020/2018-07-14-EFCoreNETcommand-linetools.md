---
title: 'EF Core .NET Command-line Tools '
date: 2018-07-14 17:51:05
tags: ABP
---

# 初始化数据库
最近想看看ABP的代码，所以下载了一份代码，然后按照官方的的流程很容易就启动起来了。然后就回家折腾一下，结果回家就没那么轻松了，家里电脑有两台，分别是mac 和win10，对mac不是很熟悉，所以优先弄了下win10上的。
但是在win10中，在Package manager console 中运行update-database的时候会出现错误，错误内容是
``` error
   It was not possible to find any compatible framework version
The specified framework 'Microsoft.NETCore.App', version '2.1.0' was not found.
```
同样的代码，在公司只有一个警告，说的也是这块版本不太匹配，但是在家直接失败，这样会导致数据库初始化失败。但是没有研究，竟然不通，就先去mac上下载了代码，安装 mariadb，这里需要提到一点，想让maradb可以远程访问需要设置密码，在apply的时候注意填写密码，否则无法在其他机器上访问这个db。

记录下本次最在意的一点，.net core 的 ef 再mac上可以使用 dotnet 命令来进行数据库的初始化操作。具体使用command的 [链接](https://docs.microsoft.com/en-us/ef/core/miscellaneous/cli/dotnet)
具体命令：
```
dotnet ef database update

```

这命令也可以在win10 中运行，并且没有错误提示。完美绕过了 pcm的错误。
还是不会写文章，只是记录下自己经历中印象略深的。