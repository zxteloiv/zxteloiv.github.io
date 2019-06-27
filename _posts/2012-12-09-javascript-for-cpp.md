---
layout: post
title:  "C++程序员的JavaScript修养"
date:   2012-12-09 19:00 +0800
tags: javascript
categories: chn
---

Javascript被称为披着C语言外衣的lisp，现在用途逐渐发展到后端，就函数式语言中来说可能是能让C语言用户看起来最亲切的。虽然在网页里跟DOM和各大浏览器对象(document,window)等结合得乱七八糟，但如果能单独剥离出来看其语言特性的话，javascript是一个自由度非常高的语言。除了google一下就能查到的API使用之外，javascript提供的语言特性不多但却足够强大，可以写出各种各样的风格，包装出各种各样的特性。

这里八卦一下，据说函数式的思路可以追溯到莱布尼兹，莱布尼兹希望可以有一个通用的的方法来解决问题：第一用一个通用的语言把问题描述出来，第二用一个通用的方法从这个通用的问题描述中求解。前者在集合论等发展之后已经解决，后者就一直到20世纪，诞生了图灵机和lambda表达式，在193X年图灵证明了这两种方法是等价的。而且目测在javascript中，匿名函数的使用要远比python2更频繁和华丽。

就个人而言，我觉得它有三个最精彩的特性：函数、闭包、原型链。

## 1. 一些基本但与C++不太一样的地方

- 一切都是对象，对象以引用方式传递。
- 支持typeof方式的反射机制，可以把对象的类型转换成’object’, ‘number’等字符串
- 变量生存期以函数为界，不以大括号为界
- 正则表达式是一等公民，即语言提供了一个正则表达式对象，而不是由函数库提供各种函数来实现的，不过它跟本文无关（泥垢

## 2. 函数及函数的调用方式

其实比较讨厌很多网贴里把函数、构造函数云云的东西扯出来的分类方法——Javascript只有一种函数，任何函数都是一样的，是从对象的亚当Object继承出来的Function对象再继承出来的一个简单对象。因为是对象，所以可以包含在其它对象中当人家的成员，也可以自己添油加醋增加方法，还可以当成变量做其他函数的传入参数，也可以作为函数返回值在其他函数执行之后动态返回。

首先要提的是this关键字，它跟指针没有一毛钱关系，只是一个表示当前执行环境的引用。这意味着如果你代码里用到this，虽然这个函数是在这个对象A里声明的，如果你在另一个对象B里也直接赋值了这个函数，那么this执行的时候就会对B的引用。

那么，一个函数代码片段用到this，具体指代的东西就会随着函数的调用方式分三种情形：

### 2.1 对象调用

```javascript
obj = {name:'world'};
obj.foo = function() { console.log('hello ' + this.name); };
obj.foo(); // this is bind to obj, print "hello world"
```

这种情形跟C++/Java一样就不说了，this指向的是这个obj对象。

### 2.2 函数调用

```javascript
var name = 'world';
var foo = function() { console.log('hello ' + this.name); };
foo();  // this is bind to global, print "hello world"

var foo2 = function() {
   var name = "cat";
   var str = foo();
   console.log(str);
}
foo2(); // still print "hello world"
```

这种直接调用一个函数，this会指向全局环境，实际上虽然这种设计不好，例如如果能打出上面foo2里定义的局部变量”cat”就好了，因为无论你在任何地方直接调用函数，this永远都是指向全局，不过倒是比较好记。

### 2.3 构造调用

```javascript
var name = 'world';
var foo = function() { console.log('hello ' + this.name); };
foo();  // "hello world"
var obj = new foo(); // "hello undefined", because of name is undefined in the new object to return

var foo2 = function() { this.name = 'cat'; foo(); this.hello = foo; };
var obj2 = new foo2(); // "hello world", foo() is directly invocated, this is always bind to global
obj2.hello(); // "hello cat", this.hello is that obj
```

构造可以视为是为了兼容C++/Java等语言的习惯而加入的，具体就是new关键字加一个函数，因为它会返回一个对象，这时this执行的时候会指向这个将要返回的新对象。可以预见，假如一个人忘记写了new，那这个函数的this很可能会把全局环境较乱，加上一大堆全局变量和函数。

### 2.4 其他

可以看到，函数调用的方式不同让函数有了不同的功能，为了this，javascript提供了call和apply两个函数对象的函数，可以人为指定this。

```javascript
var name = 'world';

var foo2 = function() { this.name = 'cat'; foo.call(this); this.hello = foo; };
var obj2 = new foo2(); // "hello cat", foo.call(this) means this is the new obj
obj2.hello(); // "hello cat", this.hello is that obj
```

call和apply区别只是call是之后顺序列出这个函数的每个形参，apply是把所有foo的参数打包成一个数组，当成apply的第二个参数。

另外还有一个bind函数，跟call的形参表一样，但可以返回一个永久把指定的对象当成this的函数。

## 3. 闭包

> **严格来说这两种闭包的数学结构不一样，一个是指对某种运算封闭，一个是屏蔽了某个仅内部才能访问的元素。不过鉴于这是一篇老文，就这样保持它原样了**

Javascript的这种形式，恰如其分地展现了离散数学里近世代数提到的闭包的含义。外界只知道一个核心功能，对这个核心函数可能使用到的外部变量完全不知情。

其实换一个说法可能就容易让C++理解：就是只知道一个class的public接口而不知道它的private成员变量。区别可能只在于C++中用户知道这个类及其私有成员的存在只是无法直接访问（如果用暴力也能强制从它的内存布局上去访问），但闭包就完全不知道外部变量的存在了。

也正是由于这个相似之处，javascript可以通过闭包去模拟一个带私有成员的类模块。

```javascript
var getObj = function() {
   var num = 0;
   return {
      increment: function(inc) { this.num += inc; },
      getValue: function() { return this.num; }
   };
};

var obj = getObj();
obj.increment(5);
console.log(obj.getValue()); // "5"
```

obj对象是执行getObj函数返回的，它带有两个成员函数，但却无法找到num变量的存在，num只存在于obj对象的闭包作用域中，目前只能通过increment和getValue两个接口访问它。

## 4. 原型链(prototype chain)

Javascript里一切都是对象，但是它们也有一个继承关系。例如Object是一切对象的祖先，它也是个对象，但是，如果直接给Object加上一个name变量，其他的对象并不会因此也能访问到name的值。

这是因为虽然javascript没有类，但却有实例，Object本身也是一个实例，这个实例有一个Object.prototype属性（也是个对象），所有的对象都继承于这个原型。给Object.name = ‘hello’只能让Object多一个属性，但Object.prototype.name = ‘world’就能让所有的对象包括Object自己都多一个’世界’，不论这些对象是在执行这句话之前还是之后创造出来的。

类似的，Array, Function等对象都是从Object.prototype继承而来的，并且给Array.prototype或Function.prototype添加新函数的话，就能给所有的数组和函数添加新功能了。

———

感觉只要熟悉了这些基本就可以无困惑地阅读javascript源码了，但是要写的话还是得翻看标准库了的。憋了一个月才把这点东西憋出来，基本上算是对自己学习javascript的这段时间做个小结和告别，以后离职了就不大会有机会用得到这些了。虽然我以前很讨厌写网站现在也是，但是也许换个角度来看这些东西就可以总结出别样的趣味来。

原型链的部分写得很挫，需要画个图才能说明白就不写了。javascript更多的复杂处在于可以写各种各样的模式，怎么写一个模块或者增加curry/uncurry等特性，这实际上相当于编程风格的范畴了，C++用户中也有很多类似的研究，这个以后慢慢总结。
