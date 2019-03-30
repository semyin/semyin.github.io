---
title: JavaScript中的原型链
date: 2019-01-31 09:54:26
tags:
---

#### JavaScript 对象的创建方式

在 JavaScript 中，创建对象的方式包括两种：  
1、对象字面量  
2、使用`new`表达式  
对象字面量是一种灵活方便的书写方式，例如:

```JavaScript
var point = {
    p:'test',
    alertP:function () {
        alert(this.p);
    }
}
```

这样就用对象字面量创建了一个对象 p，它具有一个成员变量以及一个  成员方法 `alertP`。这种写法不需要定义构造函数，因此不在本文讨论范围之内。这种写法的缺点是，每创建一个  新的对象都需要写出完整的定义语句，不便于创建大量相同类型的  对象，不利于继承等高级特性。

new 表达式是配合构造函数使用的，例如`new String('a string')`，调用内置的`String`函数构造一个字符串对象。下面我们用构造函数的方式来重新创建一个实现同样功能的对象，首先是定义构造函数，然后是用调用`new`表达式:

```JavaScript
function point () {
    this.p = 'test';
    this.alertP = function () {
        alert(this.p);
    }
}

var point2 = new point();
```

那么，在使用`new`操作符来调用一个构造函数的时候，发生了什么了呢？其实很简单，就发生了四件事：

```JavaScript
var obj = {};
obj.__proto__ = point.prototype;
point.call(obj);
return obj;
```
第一行，创建一个空的对象`obj`   
第二行，将这个对象的`__proto__`成员指向了构造函数对象的`prototype`成员对象，这是最关键的异步，具体细节将在下文描述。    
第三行，将构造函数的作用域赋给新对象，因此`point`函数中的`this`指向新对象`obj`，然后调用`point`。于是我们就给`0bj`对象赋值了一个成员变量`p`，这个成员变量的值就是`'test'`
