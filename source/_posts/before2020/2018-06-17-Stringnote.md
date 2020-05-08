---
title: String注意点
date: 2018-06-17 23:15:54
tags:
---
# 字符串换行
 ``` C#
    System.Console.WriteLine($@"Your full name is : {
                firstName} {lastName}");
 ```
    很好的点，可能不高级，但是看了觉着很好。

# using static
 
 ``` C#
    using static System.Console;
        class Program
        {
            static void Main(string[] args)
            {
                WriteLine("Hello World!");
            }
    }
 ```   
C# 6.0的新写法



