# 修复npm安装全局包/命令的权限问题

有时在全局安装包的时候会遇到<span style="color: red;">`EACCES error`</span>这种报错，这表明你对npm指定的安装全局包或者命令的目录没有写权限，可以通过下面三种方式修复这个问题：

* 修改npm指定的全局安装目录的所有者，即将目录的所有者修改为当前登陆用户
* 修改npm全局安装的默认目录
* 使用第三方包管理工具来安装Node

## 修改npm全局安装目录的所有者

要查找npm默认的全局安装目录，可通过下面的命令来查找：

```shell
$ npm config get prefix
```

对于大多数用户来说，安装目录是`/usr/local`。

**⚠️注：如果安装目录是`/usr`，请使用第二种方式(即修改npm全局安装的默认目录)来修复这个问题。**

找到安装目录后，可以使用下面的命令将该目录的所有者改成你自己：

```shell
$ sudo chown -R $(whoami) $(npm config get prefix)/{lib/node_modules,bin,share}
```

## 修改npm全局安装的默认目录

有时候修改目录所有者的方式并不合适：比如说你跟其他人共同使用一个环境时，就不能修改npm全局安装目录的所有者，因为这样做，你的问题解决了，其他人就有问题了；而且，在服务器上，你可能并没有`root`权限，所以我们可以通过修改npm默认目录的方式来修复这个问题：

```shell
$ cd
$ mkdir .git_global
$ npm config set prefix ~/.git_global

$ echo "export PATH=~/.git_global:$PATH" >> ~/.profile
$ source ~/.profile
```

上面的命令做了下面几件事情：

* 切换到当前用户的家目录
* 创建`.git_global`目录
* 设置npm的默认全局安装目录
* 在开机启动文件中设置`PATH`
* 将`~/.profile`文件立即执行一遍，这会将其中的shell环境变量重新加载一遍。如果不是立即使用npm全局安装，假设在重启电脑之后安装，则不需要这一行命令，因为系统在开机时会执行`~/.profile`文件并加载其中的shell环境变量

## 让包管理工具来解决这些问题

如果你是使用Mac OS并且第一次安装Node的话，可以使用[`homebrew`](http://brew.sh/)这个包管理工具来解决一些例如权限这种问题。

```shell
$ brew install node
```

`homebrew`可以很好帮你设置正确的权限。

## 参考

[Fixing npm permissions](https://docs.npmjs.com/getting-started/fixing-npm-permissions)

## 声明

本文部分内容来自网络，如有版权问题请联系作者。

侵删。

内容如有不恰当或错误，敬请指正。

作者邮箱：<web.taox@gmail.com>

## Author Info ✒️

* [GitHub](https://github.com/Tao-Quixote)
* Email: <web.taox@gmail.com>