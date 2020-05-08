---
title: 'c# List to Dictionary'
date: 2019-04-08 17:51:05
tags: c#
---


C#.NET 利用ToDictionary()，GroupBy()，可以将List转化为Dictionary，主需要一行代码！

首先看一下需求，已知cars，等于：
``` c#
List<Car> cars = new List<Car>(){
                new Car(1,"audiA6","private"),
                new Car(2,"futon","merchant"),
                new Car(3,"feiren","bike"),
                new Car(4,"bukon","private"),
                new Car(5,"baoma","private"),
                new Car(6,"dayun","merchant")
            };
```
1）我想以id为键，值为Car转化为一个字典idCarDict，方法如下：

    var idCarDict = cars.ToDictionary(car=>car.id);

这样保证能正确转化的前提为，id在列表中没有重复值。如果有重复的，会抛出向字典中添加重复值的异常。

2）我想以type为键，值car的List的typeDict，方法如下：

    Dictionary<string, List<Car>> typeCarDict = cars.GroupBy(car => car.type).ToDictionary(r => r.Key, r => r.ToList());

分步解释： 
第一步分组

    cars.GroupBy(car=>car.type) //返回的结果类型为： //IEnumerable<IGroup<string,car>>;
//其中string等于car.type，也就是分组的键
第二步将IEnumerable类型转化为字典，选取合适的键，

    ToDictionary(r=>r.Key,r=>r.ToList());
//r参数代表分组对象，r.Key便是car.type;
//r.ToList()操作后将分组对象转化为List对象
这种转化代码简介，比以下foreach遍历得到以car.type的字典简洁许多：
``` c#
var dict = new Dictionary<string,List<Car>>();
foreach(var car in cars)
{
  if(dict.Contains(car.type))
     dict[car.type].Add(car);
  else
    dict.Add(car.type,new List<Car>(){car}));
}
```

转的文章，希望自己能记住todictionary 的使用方法
[链接](http://www.voidcn.com/article/p-cvbngeoz-p.html)