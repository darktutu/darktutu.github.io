---
title: 多少只羊的面试小问题
date: 2018-06-28 03:32:15
tags: 面试
---

# 问题
小羊能活5岁，它在2岁，4岁的时候都会生一只小羊，5岁的时候就死亡了。
问：现在有一只刚出生的小羊(0岁),n年后有多少只羊？


# 回答

``` C#
  int zeroSheepCount = 1, sheep1Count=0 , sheep2Count=0, sheep3Count=0, sheep4Count=0, sheep5Count=0;
            for (int i = 0; i < 10; i++)
            {
                sheep5Count = sheep4Count;
                sheep4Count = sheep3Count;
                sheep3Count = sheep2Count;
                sheep2Count = sheep1Count;
                sheep1Count = zeroSheepCount;

                 var newSheep = sheep2Count + sheep4Count;
                zeroSheepCount = newSheep;
            }
            var allCount = zeroSheepCount + sheep1Count + sheep2Count + sheep3Count + sheep4Count + sheep5Count;
            Console.WriteLine(allCount);
```

常见的处理方式都会弄出来一个羊的对象，然后记录年龄，判断年龄。这样能够解决这个问题，但是羊的数量增加的很快，50年的时候内存已经增加明显，所以这个方法就不会有内存这方面的问题。