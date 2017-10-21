# 使用范围包(scoped packages)

在别的开发语言，比如`java`中，有`命名空间`(namespaces)这么一个东西，命名空间的主要作用是用来标识一个包、或者文件，以便于和其他的包、或者文件区分开来。

比如两个开发人员都开发了一个软件包，两个软件包的名字都叫`package-A`，两个包实现的功能一样，性能也差不多；你喜欢使用A开发的`package-A`，而另外一个同事喜欢使用B开发的`package-A`，所以你们两个人在同一个项目中分别引入了A和B开发的名字一样但是API不同的包。当你们想在项目中区分使用自己喜欢用的那个`package-A`包时，就可以通过`命名空间`来区分。而对于npm包，则采用了`范围`(scoped)这么一个名字，其作用和`命名空间`相同，都是用来区分名字相同但是并不是同一个包的包。

例如两个开发者分别开发了一个`my-package`，如果在使用中加上范围`@A/my-package`，`@B/my-package`，就可以指向唯一的一个包，这样就不会混淆了。范围的名字就是`@`和`/`之间的部分。

在npm中，每个已经注册的用户都有自己的范围：

```npm
@username/project-name
```

其中的范围`username`即为在[npm](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/%E4%BB%80%E4%B9%88%E6%98%AFnpm.md)上注册时的用户名。

## 使用范围包的要求

要使用范围包需要npm的版本在2.7.0之上；如果是第一次使用范围包，需要在命令行登陆：

```shell
$ npm login
Username: taoquixote
Password: 
Email: (this IS public) web.taox@gmail.com
```

在登陆时，npm会将上面的提示依次输出提示你输入需要的登陆信息。

## 初始化一个范围包

1、如果要将现有的npm包转换为范围包，只需要将`package.json`中的`name`部分改成如下格式：

```package.json
{
	"name": "@username/my-project"
}
```

2、如果是使用`npm init`初始化一个npm范围包，可以在命令行中加上**scope**选项：

```shell
$ npm init --scope=username
```

3、如果是在全局范围内一直使用一个固定的**范围(scope)**的话，可以在`.npmrc`文件中配置默认的scope：

```shell
$ npm config set scope username
```

## 发布范围包

范围包默认是私有的，但是在npm中私有包是收费的，详情请参考[Private Module](https://www.npmjs.com/features)。

npm中发布公开的范围包是不需要付费订阅的，所以如果想发布公开的范围包，可以在命令中加上`--access`选项来指定访问权，命令如下：

```shell
$ npm publish --access=public @username/project-name
```

## 使用范围包

1、使用范围包时，只需要在`package.json`文件中包名前加上范围(scope):

```package.json
{
	"dependencies": {
		"@username/project-name": "^1.0.0"
	}
}
```

2、使用命令行安装范围包：

```shell
$ npm install @username/project-name --save
```

3、Node中使用**require**加载范围包：

```javascript/Node
let sp = require('@username/project-name')
```

4、ES6中使用**import**加载范围包：

```javascript
import sp from '@username/project-name'
```

## 参考

* [Working with scoped packages](https://docs.npmjs.com/getting-started/scoped-packages)

## 声明

本文部分内容来自网络，如有版权问题请联系作者。

侵删。

内容如有不恰当或错误，敬请指正。

作者邮箱：web.taox@gmail.com。

## Author Info

* [GitHub](https://github.com/Tao-Quixote)
* Email: web.taox@gmail.com