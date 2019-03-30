---
title: ES2015中class基本语法
date: 2019-01-29 17:29:17
tags:
    -ES6
---

### 类的由来

JavaScript 语言中，生成实例对象的传统方法是通过构造函数。下面是一个例子

```JavaScript
function Point (x,y) {
    this.x = x
    this.y = y
}

Point.prototype.toString = function () {
    return '(' + this.x + ', ' + this.y + ')';
}

var p = new Point(1,2)
```

上面这种写法跟传统的面向对象语言（比如 C++和 Java）差异很大，很容易让新学习这门语言的程序员感到困惑  
ES6 提供了更接近  传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过`class`关键词，可以定义类。  
基本上，ES6`class`可以看做只是一个语法糖，他的绝大部分功能，ES5 都可以做到，新的`class`写法只是让对象原型的写法更加清晰、更像面向对象编程的语法而已。上面的代码用 ES6 的`class`改写，就是下面这样。

```JavaScript
class Point {
    constructor (x,y) {
        this.x = x
        this.y = y
    }
    toString () {
        return '(' + this.x + ',' + this.y + ')'
    }
}
```

 上面代码定义了一个”类“，可以看到里面有一个`constructor`方法，这就是构造方法，而`this`关键词则代表实例对象。也就是说，ES5 的构造函数`Point`，对应 ES6 的`Point`类的构造方法。  
`Point`类除了构造方法，还定义了一个`toString`方法。注意，定义”类“的方法的时候，前面不需要加上`function`这个关键字，直接把桉树定义放进去就可以了。另外，方法之间也不需要都好分隔，加了会报错。  
ES6 的类，完全可以看做构造函数的另一种方法。

```JavaScript
class Point {
    // ...
}
typeof Point // "function"
Point === Point.prototype.constructor  // true
```

上面的代码表明，类的数据类型就是函数，类本身就指向构造函数。  
使用的时候，也是直接对类使用`new`命令，跟构造函数的用法完全一致.

```JavaScript
class Bar {
    doStuff () {
        console.log('stuff);
    }
}

var b = new Bar();
b.doStuff()  // "stuff"
```

构造函数的`prototype`属性，在 ES6 的”类“上面继续存在。事实上，类的  所有方法都定义在类的`prototype`属性上面。

```JavaScript
class Point {
    constructor () {
        // ...
    }

    toString () {
        // ...
    }

    toValue () {
        // ...
    }
}

// 等同于

Point.prototype = {
    constructor () {},
    toString () {},
    toValue () {},
}
```

在类的实例上面调用方法，其实就是调用原型上的方法。

```JavaScript
class B {}
let b = new B()
b.constructor === B.prototype.constructor  // true
```

上面代码中，`b`是`B`类的实例，它的`constructor`方法就是`B`类原型的`constructor`方法。  
由于类的方法都定义在`prototype`对象上面，所以累的新方法可以添加在`prototype`对象上面。`Object.assign`方法可以很方便地一次向类添加多个方法。

```JavaScript
class Point {
    construtor () {
        // ..
    }
}
Object.assign(Point,prototype,{
    toString () {},
    toValue () {}
});
```

`prototype`对象的`construtor`属性，直接指向”类“的本身，这与 ES5 的行为是一致。

```JavaScript
Point.prototype.constructor === Point //true
```

另外，类的内部所有定义的方法，都是不可枚举的(non-enumerable)。

```JavaScript
class Point {
    constructor (x,y) {
        // ...
    }
    toString () {
        // ...
    }
}

Object.keys(Point.prototype)
// []
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```

上面代码中，`toString`方法是`Point`类内部定义的方法，它是不可枚举。这一点与 ES5 的行为不一致。

```JavaScript
var Point = function (x,y) {
    // ...
}
Point.prototype.toString = function () {
    // ...
}

Object.keys(Point.prototype)
// ["toString"]
Object.getOwnPropertyNames(Point.prototype)
// ["constructor","toString"]
```

 上面采用 ES5 的写法，`toString`方法就是可枚举的。

### constuctor 方法

`constructor`方法是类的默认方法，通过`new`命令生成的对象实例时，自动调用该方法。一个类必须有`constructor`方法，如果没有显示定义，一个空的`constructor`方法会被默认添加。

```JavaScript
class Point {

}
//等同于
class Point {
    constructor () {

    }
}
```

上面代码中，定义一个空的类`Point`，JavaScript 引擎会自动为它添加一个空的`construtor`方法。  
`constructor`方法默认返回实例对象（ 即`this`），完全可以指定返回另外一个对象。

```JavaScript
class Foo {
    constructor () {
        return Object.create(null);
    }
}

new Foo() instanceof Foo
// false
```

上面的代码中，`constructor`函数返回一个全新的对象，结果导致实例对象不是`Foo`类的实例。  
类必须使用`new`调用，否则会报错。 这是它跟普通构造函数的一个主要区别，后者不用`new`也可以执行。

```JavaScript
class Foo {
    constructor () {
        return Object.create(null)
    }
}

Foo()
// TypeError:Class constructor Foo cannot be invoked widthout 'new'
```

### 类的实例

生成类的实例的写法，与 ES5 完全一样，也是使用`new`命令。前面说过，如果忘记加上`new`，像函数那样调用`Class`，将会报错。

```JavaScript
class Point {
    // ...
}
//报错
var point = Point(2,3);

// 正确
var point = new Point(2,3)
```

与 ES5 一样，实例的属性除非显示定义在其本身（即定义在`this`对象上），否则都是定义在原型上（即定义在`class`上）。

```JavaScript
// 定义类
class Point {
    constructor (x,y) {
        this.x = x
        this.y = y
    }

    toString () {
        return '(' + this.x + ', ' + this.y + ')'
    }
}
var point = new Point(2,3)
point.toString() //(2,3)
point.hasOwnProperty('x') // true
point.hasOwnProperty('y') // true
point.hasOwnProperty('toString') // false
point.__proto__.hasOwnProperty('toString') //true
```

上面代码中,`x`和`y`都是实例对象`point`自身的属性（因为定义在`this`变量上），所以`hasOwnProperty`方法返回`true`，而`toString`是原型对象的属性（因为定义在`point`类是哪个），所以`hasOwnProperty`方法返回`false`。这些都与 ES5 的行为保持一致。  
与 ES5 一样，类的所有实例都共享一个原型对象。

```JavaScript
var p1 = new Point(2,3)
var p2 = new Point(3,2)
p1.__proto__ === p2.__proto__
// true
```
