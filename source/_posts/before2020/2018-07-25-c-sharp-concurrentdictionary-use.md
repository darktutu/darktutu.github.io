---
title: 'C# ConcurrentDictionary 的使用'
date: 2018-07-25 17:51:05
tags: c#
---

# 我的问题

在编码中没有注意Dictionary 是非线程安全的，以及对当前的注入的实例化的注入方式没有关注。结果在实现一个逻辑的时候在类中定义了一个 private 的 Dictionary ，结果导致有几率会导致当前县城 hang 住，我不确定为什么会导致方法 hang 住，而不是抛异常，这个问题是同事分析 dump 文件之后返回来的。所以对这个问题的根本原因还没有找到，google 了Dictionary 线程不安全的现象，也没有找到具体有人说过。自己写了test的代码，现象也是会抛异常而不是卡住。所以这个算是一个没有找到原因的现象。

无论是不是Dictionary 导致的hang， 这个地方的Dictionary会有线程安全问题是铁定的了。所以这个地方需要修正，能想到的方案有两个

## 方案1 Dictionary + lock

## 方案2 不要声明类的的私有属性

但是领导说让使用 ConcurrentDictionary，这就牵扯出来ConcurrentDictionary 的使用的问题。


# ConcurrentDictionary 出现

# ConcurrentDictionary 使用

# ConcurrentDictionary 问题
