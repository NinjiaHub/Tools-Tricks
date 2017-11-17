# Semantic Versioning

Semantic Versioning缩写为**SemVer**。因为可以语义化的来表达当前版本做了哪些修改，所以很多项目的版本管理采用Semver。Semver相比于其他命名版本号的方法，其最大的优势也是语义化这个功能。在其他的版本命名方式中，如果没有版本命名文档，即使看到版本号，并不知道这个版本做了哪些方面的修改。而使用Semver命名版本，用户一看就知道大体做了哪些方面的修改，以及当前版本是否向后兼容。

## 什么是SemVer

SemVer由三部分组成`x.y.z`这种版本格式：

* `x`：表示主版本号`Major`，可以不向后兼容，比如API修改
* `y`：表示较小的修改`Minor`，向后兼容，常用于表示新特性的添加
* `z`：补丁`Patch`，常用于表示bugfix

所以Semver版本号的各部分分别表示`Major.Minor.Patch`。

## SemVer是如何工作的

SemVer对软件维护开发人员的要求是要在正确的时间更新版本号中正确的部分。其实，使用SemVer做版本管理时确认要修改版本号的哪个部分也不是一件困难的事情：

* 如果修改是很多bugfix，那么可以认定这次修改是一个补丁，则应该修改`z` 或者说 `Patch`部分
* 如果以向后支持的方式实现了新的特性，这种修改叫做`Minor`版本，应该修改`y` 或者说 `Minor`部分
* 如果实现了新的特性、大版本优化，而且有可能不兼容现存的API，即不向后兼容，这种叫做`Major`版本，应该修改`x` 或者说 `Major`部分

## 牢记

既然知道了SemVer是什么东西，接下来就是要记住下面两件事情：

### 版本号从0.1.0开始

所有以SemVer做版本管理的软件的版本号都应该是从`0.1.0`开始，即从一个Minor修改开始，而不是从`0.0.1`开始。因为当我们开始做一个软件的时候，刚开始肯定是加入一些功能，而不会在软件的刚开始就做bugfix和加入补丁，这从逻辑上来说也是讲不通的，因此，第一个版本号应该是从`0.1.0`开始。

### 1.0.0之前都是开发阶段

在软件的开发过程中，总会有那么一个阶段我们会自己问自己：什么时候发布一个官方主版本(即1.0.0版本)？

下面这两件事情或许可以帮助你回答这个问题：如果你的软件已经在生产环境中使用，或者已经有其他用户在依赖你的软件了，那么是时候发布1.0.0版本了；如果在当前的开发中，很多时候都会担心要修改当前的API或者不兼容以前的老版本，那么是时候发布1.0.0版本了。

在1.0.0版本之前，不要担心会影响原来的API，也不要担心会影响别的事情，做需要做的修改，只有这样，在1.0.0版本发布的时候，你的软件才会更加稳定。在1.0.0这个官方稳定版本发布之后，增加新功能时再考虑更多的事情，而在1.0.0之前，所有的工作都是为了实现想要的功能。

## 预发布版本

在发布主版本之前，开发者需要做大量的工作，包括一遍又一遍地测试，以保证所有的功能都是没问题的。这个时候，可以考虑采用预发布版本。

在SemVer中，可以通过在使用`Major.Minor.Patch-alpha.x`这种格式来发布预发布版本。例如，一个构建好的版本在还没有完全测试完之前，可以称为`1.0.0-alpha.1`版本；如果又构建了一个新的版本，那么这个版本可以称为`1.0.0-alpha.2`版本，依次类推，直至`1.0.0`版本发布。

## 版本范围

在SemVer中，可以使用`~`和`^`来表示允许更新的版本范围。`~`表示允许patch版本的更新，`^`允许Minor版本的更新。在Node.js中，官方使用[node-semver](https://github.com/npm/node-semver)来做Node.js中的SemVer版本号解析器，其中最简单的规则如下：

* `~1.2.5`表示的版本范围：仅允许更新修订号，保持主版本号和次版本号不变，即>=1.2.5 且 <1.3.0
* `^2.8.9`表示的版本范围：允许修改次版本号和修订号，保持主版本号不变，即>=2.8.9 且 <3.0.0
* `3.7.3`表示的版本范围：没有版本范围，只允许下载`3.7.3`这个特定版本

但是[node-semver](https://github.com/npm/node-semver)支持数量、类型众多的版本号规则，而且在`Major.Minor.Patch`中有缺席版本号时，规则又有所不同；但是在日常的开发中，我们通过`--save`和`--save-dev`在`package.json`中记录的依赖文件的版本号，`Major.Minor.Patch`一般都是齐全的，所以不涉及缺席版本号的情况，只要记住上面三种情况即可。

如果自己开发npm包或者Node模块，想要了解更多SemVer版本号具体guize，可以参考👉Node SemVer版本号解析器[npm-semver](https://docs.npmjs.com/misc/semver)和SemVer官方文档[Semantic Versioning 2.0.0](http://semver.org/)。

## 参考

* [Semantic Versioning: Why You Should Be Using it](https://www.sitepoint.com/semantic-versioning-why-you-should-using/)
* [Semantic versioning and npm](https://docs.npmjs.com/getting-started/semantic-versioning)
* [Semantic Versioning 2.0.0](http://semver.org/)

## 声明

本文部分内容来自网络，如有版权问题请联系作者。

侵删。

内容如有不恰当或错误，敬请指正。

作者邮箱：<web.taox@gmail.com>

## Author Info ✒️

* [GitHub](https://github.com/Tao-Quixote)
* Email: <web.taox@gmail.com>