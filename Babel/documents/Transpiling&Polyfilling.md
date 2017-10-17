# Transpiling vs Polyfilling

今天在看项目的时候突然产生一个疑问：既然`babel`是用来转译es6(及其之后的)语法的，那么`babel-polyfill`是用来做什么的？如果是一部分放在`babel presets`中来实现转译，另外一部分放在`babel-polyfill`实现转译，那么这么做的意义是什么？

带着疑问，先打开[`babel-polyfill`](https://babeljs.io/docs/usage/polyfill)在官网的文档页面，里面的解释说：

> This will emulate a full ES2015+ environment and is intended to be used in an application rather than a library/tool. This polyfill is automatically loaded when using babel-node.

> This means you can use new built-ins like Promise or WeakMap, static methods like Array.from or Object.assign, instance methods like Array.prototype.includes, and generator functions (provided you use the regenerator plugin). The polyfill adds to the global scope as well as native prototypes like String in order to do this.

简单翻译一下就是：

> babel-polyfill模仿出一个ES2015+的环境，这样我们就可以使用在ES2015+中新出现的内嵌的对象或者方法，例如`Promise`, `WeakMap`, 静态方法`Array.from`, `Ojbect.assign`, 实例具有的方法`Array.prototype.includes`等。`polyfill`会在全局作用域中添加对象或者方法。

## transpiling

如果只看上面这段，还是不清楚`babel-polyfill`跟`babel presets`所做工作的具体差别，所以我们来看下面的例子：

假设我写了下面这样一段代码：

```javascript
class Foo {
  generator (arr) {
    this.arr = arr
  }
  
  hasItem (item) {
   return this.arr.includes(item) 
  }
}
```
经过Babel转译后：

```javascript
"use strict";

var _createClass = function () { function defineProperties(target, props) { for (var i = 0; i < props.length; i++) { var descriptor = props[i]; descriptor.enumerable = descriptor.enumerable || false; descriptor.configurable = true; if ("value" in descriptor) descriptor.writable = true; Object.defineProperty(target, descriptor.key, descriptor); } } return function (Constructor, protoProps, staticProps) { if (protoProps) defineProperties(Constructor.prototype, protoProps); if (staticProps) defineProperties(Constructor, staticProps); return Constructor; }; }();

function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

var Foo = function () {
  function Foo() {
    _classCallCheck(this, Foo);
  }

  _createClass(Foo, [{
    key: "generator",
    value: function generator(arr) {
      this.arr = arr;
    }
  }, {
    key: "hasItem",
    value: function hasItem(item) {
      return this.arr.includes(item);
    }
  }]);

  return Foo;
}();
```

对比转译前后的代码可知，Babel只是将es6中的新增的语法syntax进行了转译，而浏览器中原本不存在对象、静态方法和原型连的方法，Babel不会进行转译，只是原样进行输出。

从Babel的特性可以看出，Babel本质上是一个transpiler(具体解释可以看下面的注释部分)，即 源码-源码编译器，将一种语言的源码解释翻译为另外一种语言的源码(也可能是同一种语言的不同语法)。只会对语法进行编译，而不会对环境中不存在的对象、方法等进行补充。

Babel默认只转换新的JavaScript句法（syntax），而不转换新的API，比如Iterator、Generator、Set、Maps、Proxy、Reflect、Symbol、Promise等全局对象，以及一些定义在全局对象上的方法（比如Object.assign）都不会转码，所以如果要使用新的的API，则需要使用polyfill。

## polyfill

从transpiling部分可以看出，如果要使用新的API，则需要使用polyfill来给环境加上特定新API。有时候我们可能不知道是要使用transpiling还是使用polyfill，可以参照下面的表格：

![polyfill_transpiling](https://cdn-images-1.medium.com/max/1600/1*zACTklx4k3MvtLI1B2XGtg.png)

注：上面的表格来自[Polyfills: everything you ever wanted to know, or maybe a bit less](https://hackernoon.com/polyfills-everything-you-ever-wanted-to-know-or-maybe-a-bit-less-7c8de164e423)

在平时的开发过程中，我们应该知道什么时候需要使用transpiling和什么时候使用polyfill，以免发生不必要的麻烦。

babel-polyfill使用的使用可以参考Babel官网[babel polyfill文档](https://babeljs.io/docs/usage/polyfill/)。

## 其他

在刚开始接触es6及Babel时，以为polyfill是Babel中特有的东西；后来发现polyfill其实是一个通用的东西，不局限于语言；在任何语言中使用现有功能添加环境中不存在的方法、对象、静态方法等的行为，都可以称为polyfill。

**本文主要目的在于说明Babel和babel-polyfill所做工作的区别，在开发中具体的设置，请参考官方文档和其他博文。**

## 注释
* 转译：这里使用`转译`来表示代码从es6语法转换为es5语法的这一行为并不恰当，这种行为应该算是一种编译。
* transpiler：A source-to-source compiler, transcompiler or transpiler is a type of compiler that takes the source code of a program written in one programming language as its input and produces the equivalent source code in another programming language. A source-to-source compiler translates between programming languages that operate at approximately the same level of abstraction, while a traditional compiler translates from a higher level programming language to a lower level programming language. For example, a source-to-source compiler may perform a translation of a program from Pascal to C. An automatic parallelizing compiler will frequently take in a high level language program as an input and then transform the code and annotate it with parallel code annotations (e.g., OpenMP) or language constructs (e.g. Fortran's forall statements).

## 参考

* [Babel polyfill? What is that?](https://stackoverflow.com/questions/31122193/babel-polyfill-what-is-that)
* [What does babel-polyfill do?](https://www.quora.com/What-does-babel-polyfill-do)
* [Polyfills: everything you ever wanted to know, or maybe a bit less](https://hackernoon.com/polyfills-everything-you-ever-wanted-to-know-or-maybe-a-bit-less-7c8de164e423)
* [Babel 入门教程](http://www.ruanyifeng.com/blog/2016/01/babel.html)

## 声明

本文部分内容参考自网络，如有版权问题请联系作者。如内容有错误或者不恰当的地方，敬请指正。

侵删。

作者邮箱：web.taox@gmail.com。