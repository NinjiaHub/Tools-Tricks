# 包和模块(Packages and Modules)

想要深入一种语言的生态进行更深入的学习，其中的一个关键就是学习该生态的专有名词。Node.js和npm中的**包**和**模块**非常容易混淆，而Node.js和npm对两者有非常明确的定义。我们将在本文中讨论这些定义，使其更加清晰，并且解释一些特定的默认文件为什么要如此命名。

## 快速摘要

* **包**是一个通过**package.json**文件描述的文件或者目录。具体实现方式有很多种，更多信息，请看下面的[什么是包？](#what-is-a-package)部分。
* **模块**是指任何可以通过Node.js中的**require()**加载的文件或者目录。同样，也有很多种配置方式来实现这种定义，更多信息，请看下面的[什么是模块？](#what-is-a-module)部分。

## <span id="what-is-a-package">什么是包？</span>

包是下列情况中的任意一种：

* a) 通过**pacakge.json**文件描述的、包含一个可执行程序的目录；
* b) 一个包含(a)的、压缩过的tarball；
* c) 一个指向(b)的url；
* d) 一个以**\<name>@\<version>**模式发布在注册表中的(c)
* e) 一个指向(d)的**\<name>@\<tag>**
* f) 一个有**latest**标签的**\<name>**
* g) **git url**，在使用`clone`命令克隆道本地后，解析结果是(a)

以上这些**包**的可能性，遵循的设计原则是即使没有将自己的包发布到公共的注册表，你依然可以在使用npm的过程中获得很多便利，例如：

* 你只是想写一个node程序
* 或者在将你的包打包为一个tarball之后，你想在其他的地方轻易地安装这个包

Git url可以是下面任何一种形式：

```shell
git://github.com/user/project.git#commit-ish
git+ssh://user@hostname:project.git#commit-ish
git+http://user@hostname/project/blah.git#commit-ish
git+https://user@hostname/project/blah.git#commit-ish
```

上面的**commit-ish**可以是提供给**git checkout**作为参数的tag、sha或者branch中的任意一种形式。**commit-ish**的默认值为**master**。

## <span id="what-is-a-module">什么是模块？</span>

**包**是Node.js程序可以通过**require()**加载的任何文件或者目录。下面是所有可以作为模块被加载的例子：

* 一个包含**package.json**文件、且该文件包含**main**属性的目录
* 一个包含**index.js**文件的目录
* 一个JavaScript文件

### <span id="most-npm-packages-are-modules">大部分npm包都是模块</span>

通常，在Node.js程序中使用的npm包是通过**require**被加载的，因此这些npm包是Node.js模块。但是，并没有要求一个npm包必须是模块。

其中一些包，例如**cli**包，只包含一个可执行的命令行接口，并不提供**main**属性给Node.js程序使用。这些包就不是模块。

几乎所有npm包(至少那些事Node程序的包)都在内部包含很多模块(因为它们中每一个通过**require()**加载的文件都是模块)。

在Node程序的语境中，**包**也指代从一个文件中加载的内容。例如，在下面的例子中：

```shell
var req = require('request')
```

我们可以说“变量req是指**request**模块”。

## <span id="file-and-directory-names-in-the-nodejs-and-npm-ecosystem">Node.js和npm生态中的文件名和目录名</span>

为什么是**node_modules**目录，**package.json**文件？为什么不是**node_packages**或者**module.json**？

**package.json**文件定义了包。(详情请查看上面的[什么是包？](#what-is-a-package))

**node_modules**是Node.js程序查找包的地方。(详情请查看上面的[什么是模块？](#what-is-a-module))

例如，如果你在**node_modules**目录下创建了一个**foo.js**文件`node_modules/foo.js`，然后在Node程序中使用了`var f = require('foo.js')`，这时Node在执行这条语句时会从**node_modules**目录中加载**foo.js**这个模块。然而，在这个例子中**foo.js**并不是一个包，因为该文件没有描述／定义它的**package.json**文件。

或者，如果你创建了一个包，但是其中并没有**index.js**文件或者一个包含**main**属性的**package.json**，这个包并不能算是一个模块。即使该**包**被安装在了**node_modules**目录中，它也不能作为**require()**的参数。

## 原文链接

* [Packages and Modules](https://docs.npmjs.com/how-npm-works/packages)

## 声明

本文翻译源内容来自网络，即NPM官方文档，如有版权问题请联系译者。

侵删。

内容如有不恰当或错误，敬请指正。

作者邮箱：<web.taox@gmail.com>

## Author Info

* [GitHub](https://github.com/Tao-Quixote)
* Email: <web.taox@gmail.com>
