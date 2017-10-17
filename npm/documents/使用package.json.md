# 使用package.json

为了方便管理本地已经安装的npm包和项目依赖，可以使用`package.json`文件。使用`package.json`文件管理npm包的优点：

* 1、可以作为项目依赖的文档
* 2、可以使用[Semantic versioning](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/npm%E4%BB%A5%E5%8F%8ASemver.md)规则来声明项目允许使用的npm包的版本
* 3、便于分享，在分享自己的项目时，不需要在项目中保留npm包，只需要在项目根目录(推荐)中保留一个`package.json`文件，其他人只要在`package.json`文件所在的目录运行`npm install`就可以安装所有的项目依赖了

## 文件要求

一个`package.json`文件最少要有`name`和`version`相关信息：

* name
	* 全部为小写字母
	* 不允许有空格
	* 可以使用短横线(-)和下划线(_)
* version
	* 格式 x.x.x
	* 遵循[Semantic versioning](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/npm%E4%BB%A5%E5%8F%8ASemver.md)规则

```node
{
	"name": "package_name",
	"version": ""
}
```

## 创建package.json

npm可以通过运行`npm init`在命令行创建`package.json`文件，在执行`npm init`之后，npm会提问一系列的问题，然后根据输入来创建`package.json`文件。

```shell
$ npm init
This utility will walk you through creating a package.json file.
It only covers the most common items, and tries to guess sensible defaults.

See `npm help json` for definitive documentation on these fields
and exactly what they do.

Use `npm install <pkg> --save` afterwards to install a package and
save it as a dependency in the package.json file.

Press ^C at any time to quit.
name: (npm) package_name
version: (1.0.0) 1.3.2
description: 

...
```

`npm init`命令有一个可选项`[-f|--force|-y|--yes]`，当在命令后使用该选项时，npm不会问任何问题，会在当前目录下创建一个默认的`package.json`文件。

```shell
$ npm init -f
npm WARN using --force I sure hope you know what you are doing.
Wrote to /Users/taoxin/workspace/alpha/npm/package.json:

{
  "name": "npm",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}
```

`package.json`字段释义：

* name：当前目录的名字
* version：版本号
* description：准确&简短的描述
* main：总是index.js，主入口文件
* scripts：可使用Node执行的命令，默认创建一个test
* keywords：关键字，默认为空
* author：作者
* license：许可

可依通过下面的命令设置一些有用的默认选项：

```shell
> npm set init.author.email "wombat@npmjs.com"
> npm set init.author.name "ag_dubs"
> npm set init.license "MIT"
```

#### 注意

如果`package.json`文件中没有设置description字段，npm会使用`README.md`或者`README`文件中的第一行作为项目/包的描述，这个描述会在用户使用`npm search`或者在[npm官网](https://www.npmjs.com/)搜索包的时候用来匹配用户输入的信息，所以请认真地填写description字段的内容，以便于其他用户查询到你的包。

## 具体说明项目依赖的npm包

对于项目中使用的npm包，在package.json文件中作具体说明时可以分为两类：

* dependencies：生产环境中要使用的npm包
* devDependencies：只在本地开发环境中要使用的npm包

对于项目中使用的npm包，可以通过手动修改`package.json`文件来保存npm包信息，也可以安装时在命令中通过使用`--save`或者`--save-dev`让npm自动修改`package.json`文件以达到保存npm包信息的目的。

### --save && --save-dev

对于要在生产环境中使用的npm包，可通过下面的命令安装并保存npm包信息到`package.json`文件中的**dependencies**部分：

```shell
$ npm install <package_name> --save
```

而对于只需要在本地开发过程中使用的npm包，可以通过下面的命令安装并保存npm包信息到`package.json`文件中的**devDependencies**部分：

```shell
$ npm install <package_name> --save-dev
```

### 手动修改package.json

```Node
# package.json
{
	"name": "package_name",
	"version": "1.1.1",
	"dependencies": {
		"package_a": "~1.3.3"
	},
	"devDependencies": {
		"package_b": "^1.4.8"
	}
}
```

正常情况下，推荐使用`--save` 和 `--save-dev`实现记录包信息到package.json文件的目的，如果在安装时忘记了，可以通过手动修改来记录，不过比较麻烦。

手动修改`package.json`文件时，请遵循[SemVer规则](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/npm%E4%BB%A5%E5%8F%8ASemver.md)来书写npm包的版本号。

## 根据package.json安装项目依赖

如果你从github或者公司内部的git仓库中刚clone了一个新的项目，如果项目中有`package.json`文件时，安装项目依赖的npm包是一件非常简单的事情：

```shell
$ npm install
```

在命令行中执行上面的命令，npm会自己解析`package.json`文件，然后根据`dependencies` 和 `devDependencies`中记录的npm包的信息来下载跟版本号匹配的npm包。

## 参考

* [Using a package.json](https://docs.npmjs.com/getting-started/using-a-package.json)

## 声明

本文部分内容来自网络，如有版权问题请联系作者。

侵删。

内容如有不恰当或错误，敬请指正。

作者邮箱：web.taox@gmail.com。