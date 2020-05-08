---
title: c#异步 await/async
date: 2020-05-07 10:34:05
tags: 
- c#
categories:
- c#
---

在面试中被两次问过 await/async，让回答一下这两个关键字。之前确实没有是实操使用过这两个关键字，知道是来来完成异步操作的，倒是用过 .GetAwaiter().GetResult().所以面试中很没底气，没有完整的回答这个问题。所以打算整理一下这个知识点。

[走进异步编程的世界 - 开始接触 async/await](https://www.cnblogs.com/liqingwen/p/5831951.html)
[走进异步编程的世界 - 剖析异步方法（上）](https://www.cnblogs.com/liqingwen/p/5844095.html)
[走进异步编程的世界 - 剖析异步方法（下）](https://www.cnblogs.com/liqingwen/p/5866241.html)
[走进异步编程的世界 - 在 GUI 中执行异步操作](https://www.cnblogs.com/liqingwen/p/5877042.html)

这四篇有个详细的介绍，但是感觉并不是很理解。还是要找点文章自己也实际编码一下，好好总结。