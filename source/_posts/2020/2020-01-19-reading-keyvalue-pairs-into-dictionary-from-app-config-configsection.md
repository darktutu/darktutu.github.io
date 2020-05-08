---
title: c# – 从app.config configSection中读取键值对到字典
date: 2020-01-19 10:34:05
tags: c# .net
---


``` xml
    <configSections>
        <section  name="ReplaceStr" type="System.Configuration.DictionarySectionHandler" />
    </configSections>

    
  <ReplaceStr>
    <add key="(" value="0"/>
    <add key=")" value="0"/>
  </ReplaceStr>

```

``` c#
 var section = (ConfigurationManager.GetSection("ReplaceStr") as System.Collections.Hashtable)
                  .Cast<System.Collections.DictionaryEntry>()
                  .ToDictionary(n => n.Key.ToString(), n => n.Value.ToString());
```