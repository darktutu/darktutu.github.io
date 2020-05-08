---
title: 'C# ConvertTimeToUtc error'
date: 2019-03-27 17:51:05
tags: c#
---

记录下见过的error,
产品环境的代码不一样但是和这个运行的结果是一样的
ConvertTimeToUtc
``` c#
var tz = TimeZoneInfo.FindSystemTimeZoneById("W. Europe Standard Time");
var swedishTime = TimeZoneInfo.ConvertTime(DateTime.UtcNow, tz);

System.ArgumentException: The conversion could not be completed because the supplied DateTime did not have the Kind property set correctly.

```

解决方式是 直接用了 new data(); 不适用utcnow


