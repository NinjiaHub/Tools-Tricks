# 局部安装npm包

根据我们如何使用npm包，有两种方式来安装npm包：全局安装和局部安装。如果想将一个npm包作为像`vue CLI`这样的命令行工具来使用，最好全局安装这个npm包；而如果你只是想将这个npm包作为一个项目的依赖来使用的话，推荐将npm包局部安装在项目中，因为对于同一个npm包，不同的项目可能在使用该包的不同版本，如果全局安装的话，不同项目在安装npm包时由于使用的版本不同可能会产生冲突或者覆盖，所以推荐将项目使用的npm包局部安装在项目中。

本文主要介绍局部安装相关的知识。

## 安装

npm包可以通过如下命令来下载：

```shell
$ npm install <package_name>
```

如果执行该命令的目录中没有`node_modules`目录时，npm会在该目录中创建一个`node_modules`目录，然后将下载的npm包放在`node_modules`目录中。

### 测试

可以使用下面的方法测试npm包是否正确安装了：

* 1、查看执行`npm install`命令的目录下是否存在`node_modules`目录
* 2、执行下面的命令，查看`node_modules`目录中是否有以你安装的包的名字命名的目录

```shell
$ ls node_modules
```

**注：上面的方法可以在Unix、OSX、Debian等系统上使用；Windows系统请使用`dir node_modules`。**

## 安装包的版本

一个npm包可能有很多个版本，在执行`npm install <package_name>`安装包时遵循以下规则：

* 1、本地目录中没有`package.json`文件，如果`package_name`标识的npm包存在，则安装该包已经发布的最新版本
* 2、本地目录中有`package.json`文件，则根据[SemVer规则](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/SemVer.md)来安装符合规则版本的安装包

## 使用已安装的包

假设我们通过下面的命令安装了lodash：

```shell
$ npm install lodash
```

则可以在js文件中通过使用Node.js支持的`require`关键字来引入依赖包并且使用：

```js
# index.js

var lodash = require('lodash');
 
var output = lodash.without([1, 2, 3], 1);
console.log(output);
```

通过Node运行index.js文件，可以得到输出结果`[2, 3]`。

**注：Node有一套自己查找依赖包的规则，详情请戳👉[Node查找依赖包/库的规则](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/Node%E6%9F%A5%E6%89%BE%E4%BE%9D%E8%B5%96%E5%8C%85-%E5%BA%93%E7%9A%84%E8%A7%84%E5%88%99.md)**

## 参考

* [Installing npm packages locally](https://docs.npmjs.com/getting-started/installing-npm-packages-locally)
* [Semantic versioning and npm](https://docs.npmjs.com/getting-started/semantic-versioning)

## 声明

本文部分内容来自网络，如有版权问题请联系作者。

侵删。

内容如有不恰当或错误，敬请指正。

作者邮箱：web.taox@gmail.com。