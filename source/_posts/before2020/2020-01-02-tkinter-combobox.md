---
title: Python Tkinter  Combobox 
date: 2020-01-02 10:34:05
tags: Python,Tkinter
---


###基础

`Combobox` 包含在 'ttk' 中，所以需要先引入 ttk

Combobox 支持的属性：
- values 或者 value :
- state : readonly 
- current: 选中的内容
- ComboboxSelected ：绑定的选择事件




```python
import tkinter
from tkinter import ttk # 导入ttk模块，因为下拉菜单控件在ttk中

root = tkinter.Tk()
root.title("root")
root.geometry("300x200+10+20")

xVar = tkinter.StringVar()

# 创建下拉菜单
cmb = ttk.Combobox(root,textvariable=xVar)
cmb.pack(side=tkinter.TOP)

# 设置下拉菜单中的值
cmb['value'] = ('上海','北京','天津','广州')

# 设置默认值，即默认下拉框中的内容 默认值中的内容为索引，从0开始
cmb.current(2)

# 执行函数
def func(event):
    lVar.set('label1'+cmb.get())
    label2.config(text='label2'+xVar.get())
cmb.bind("<<ComboboxSelected>>",func)

lVar = tkinter.StringVar()

label1 = tkinter.Label(root,textvariable=lVar,)
label1.pack(side=tkinter.TOP)

label2 = tkinter.Label(root,bg='green')
label2.pack(side=tkinter.TOP)

root.mainloop()
```


```python

```
