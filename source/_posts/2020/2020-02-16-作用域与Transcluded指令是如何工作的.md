
---
title: 作用域与Transcluded指令是如何工作的
date: 2020-02-16 10:34:05
tags: angularjs
---

指令最常见的一个应用是创建可重用构件。

下面的为代码展示了一个简单版本的对话框指令的使用。

``` html
<div>
  <button ng-click="show=true">show</button>
 
  <dialog title="Hello ."
          visible="show"
          on-cancel="show = false"
          on-ok="show = false; doSomething()">
     Body goes here:  is .
  </dialog>
</div>
```
点击 "show" 按钮会打开对话框。对话框会有一个和 username 绑定的标题，同时会有一个主体，这个主体我们是通过在对话框指令定义中的模板通过 transclude 插入的。

下面是 dialog 指令中的模板属性：
``` html
<div ng-show="visible">
  <h3></h3>
  <div class="body" ng-transclude></div>
  <div class="footer">
    <button ng-click="onOk()">Save changes</button>
    <button ng-click="onCancel()">Close</button>
  </div>
</div>
```
上面这个指令的模板还不能适当地渲染，除非我们施上一些魔法。

第一个问题是解决对话框模板中需要的 title 数据。而我们希望模板中的 title 和指令 <dialog> 被使用时的 title 属性一致（也就是 "Hello {{username}}"）。而且，模板中的按钮会去调用作用域中的 onOk 和 onCancel 两个函数，而这两个函数的来源也在指令的属性中有定义，要解决的就是映射的问题了。为了解决这个映射问题，我们使用本地 scope 创建本地变量（模板中需要的数据和函数）和外部变量（指令中已有的属性）映射：
```js
  scope: {
    title: '@',             // the title uses the data-binding from the parent scope
    onOk: '&',              // create a delegate onOk function
    onCancel: '&',          // create a delegate onCancel function
    visible: '='            // set up visible to accept data-binding
  }
  ```
在指令的作用域创建本地变量会产生两个问题：

独立的作用域 - 如果用户（译注：使用对话框指令的开发者）在使用 dialog 指令时忘记设置 title 属性，那么对话框指令的模板中的 title 解析时则会去绑定父级作用域中的同名属性。这是完全不可预测的，也不是我们希望看到的。

transclusion - 通过引用包含的DOM节点可以看到指令的本地变量，而这本地变量有可能会重写掉一些 transclusion （引用包含）中数据绑定的同名属性。在上述例子中，比如像指令所在的 scope 中的 title 属性就重写了在 transclusion （引用包含）的作用域的 title 属性（译注：这里 Body goes here: {{username}} is {{title}}. 是通过 transclusion 插入到 dialog 指令模版中拥有 ng-transclude 属性的div中，这样它里面的 title 插值就会被 dialog 本地的 title 值改写 ）

为了解决缺少隔离的问题，指令会声明一个 isolated 作用域。一个隔离的作用域不会通过基于原型的方式继承它的父级作用域，所以我嗯就不用担心会有属性被意外改写的情况了。

但是，独立的作用域引来了另外一个问题：如果一个 transcluded（引用包含的） DOM节点是一个指令独立作用域的孩子节点的话，那么它不会绑定到任何数据（译注：像我们上面例子中的情况，就属于绑定不到数据）。出于此，在指令为本地变量创建出独立的作用域之前，我们需要声明 transcluded（引用包含的）作用域是原始作用域（也是独立作用域的父级）的孩子。这样，引用包含创建的作用域就和独立作用域拥有同样的父级，也就是说它们是兄弟作用域。

这会让一切看起来意想不到的复杂，但至少它让指令（控件）的用户和开发者不会那么难以接受。

因此，我们上面例子中指令作用域的声明，最后开起来是这样子的：
``` js
transclude: true,
scope: {
    title: '@',             // the title uses the data-binding from the parent scope
    onOk: '&',              // create a delegate onOk function
    onCancel: '&',          // create a delegate onCancel function
    visible: '='            // set up visible to accept data-binding
},
restrict: 'E',
replace: true
```
译注：最后关于引用包含的作用域这边是一个例子，更好帮你理解 transclue

