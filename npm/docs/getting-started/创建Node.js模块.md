# 创建Node.js模块

Node.js模块也是可以发布到npm的包的一种。当开始创建一个新的Node.js模块时，最好从`package.json`文件开始。关于`package.json`文件的使用，请戳👉[使用package.json](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/%E4%BD%BF%E7%94%A8package.json.md)。

在创建包的时候，如果没有特殊需求，可以使用`package.json`中的**main**来指定该包的入口文件，约定俗成的入口文件为模块根目录下的`index.js`文件。

如果想在`package.json`文件中添加作者信息，推荐在**author**中使用如下格式来设置作者信息(邮箱和网站是可选项，可以不填写)：

```text
Your Name <email@email.com> (http://example.com)
```

在`package.json`文件创建完之后，就应该创建该模块的主入口文件了，通常默认的主入口文件为模块根目录下的`index.js`文件。

```shell
module-A
	|- index.js
	|- package.json
```

通常在`index.js`文件的末尾通过关键字**export**来导出一个对象，该对象中包含该模块想要想外部暴露的方法或者属性；当在别的js中想要使用该模块时，可以通过**require**来加载该依赖模块并使用其中的方法。

```javascript
# index.js

exports.printMsg = () => {
	console.log('some words')
}
```

```javascript
# a.js
let printMsg = require('./module-A').printMsg

printMsg()

// some words
```

Node在运行目标js代码时，会根据[Node查找依赖包/库的规则](https://github.com/NinjiaHub/Tools-Tricks/blob/master/documents/getting-started/npm/Node%E6%9F%A5%E6%89%BE%E4%BE%9D%E8%B5%96%E5%8C%85-%E5%BA%93%E7%9A%84%E8%A7%84%E5%88%99.md)在系统中查找依赖的npm包，当上面的a.js代码运行输出`some words`时，表示Node模块书写是正确的。上面的写法只是在使用本地的Node模块，还可以将自己的Node模块推送到npm(Node官方的模块管理仓库中)中，供别人下载使用。如果根据实际的情况，是公司内部相关的代码，应该根据实际情况再决定是否推送到npm，因为npm上的代码都是开源的。

## 参考

* [Creating Node.js modules](https://docs.npmjs.com/getting-started/creating-node-modules)

## 声明

本文部分内容来自网络，如有版权问题请联系作者。

侵删。

内容如有不恰当或错误，敬请指正。

作者邮箱：web.taox@gmail.com。

## Author Info

* [GitHub](https://github.com/Tao-Quixote)
* Email: web.taox@gmail.com
