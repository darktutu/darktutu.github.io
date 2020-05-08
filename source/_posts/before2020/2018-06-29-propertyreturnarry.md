---
title: 避免属性返回数组
date: 2018-06-29
tags: .Net设计规范 
---

# 问题

通常返回内部数组的副本是必需的，这样用户就无法改变对象的内部状态。但这会导致代码的效率十分低下。

下面的例子中，在循环的每次迭代过程中 `Employees` 属性被访问了两次。下面这一小段代码就会产生2n+1个副本：

    Company microsoft = GetCompanyData("MSFT");
    for(int i = 0 ; i < microsoft.Employess.Lengthh; i++){
        if(microsotf.Employess[i].Alias == "kcwalina"){
        ...
        }
    }
    

# 解决方案

这个问题可以通过以下两种方式解决。

一种方式是把属性改为方法，这可以告诉调用者该操作不只是访问内部字段，还有可能在每次调用时创建一个数组。这样一来，用户很可能只调用一次该方法，把结果暂时保存起来，并对保存的结果进行操作。

    Company microsoft =GetCompanyData("MSFT");
    Employees[] employees = microsoft.GetEmployees();
    for(int i = 0 ; i < employees.Length; i++){
        if(employees[i].Alias == "kcwalina"){
        ...
        }
    }

另一种方式是把属性的返回值改成集合而不要用数组。我们可以用'REadOnlyCollection<T>'以只读的形式来公开提供对私有数据的访问。或者，我们也可以用'Collection<T>'的子类来提供受控的读写访问，这样在用户的代码修改集合时，我们就能得到通知。

    pulic ReadOnlyCOllection<Employees> Employees{
        get {return roEmployees; }
    }
    private Employee[] employees;
    private ReadOnlyCollection<Employees> roEmployees;




