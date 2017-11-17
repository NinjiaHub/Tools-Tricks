# npm全局安装包的管理

根据我们如何使用npm包，有两种方式来安装npm包：全局安装和局部安装。如果想将一个npm包作为像vue CLI这样的命令行工具来使用，最好全局安装这个npm包；而如果你只是想将这个npm包作为一个项目的依赖来使用的话，推荐将npm包局部安装在项目中，因为对于同一个npm包，不同的项目可能在使用该包的不同版本，如果全局安装的话，不同项目在安装npm包时由于使用的版本不同可能会产生冲突或者覆盖，所以推荐将项目使用的npm包局部安装在项目中。

本文主要介绍全局安装相关的知识。

## 全局安装

全局安装npm包，只需要在命令行执行：

```shell
$ npm install -g <package_name>
```

如果遇到<span style="color: red;">`EACCES error`</span>，请参考[修复npm安装全局包/命令的权限问题](https://ninjiahub.github.io/Tools-Tricks/npm/docs/getting-started/npm%E6%9D%83%E9%99%90%E9%97%AE%E9%A2%98)。在Unix-like系统中，也可以尝试使用`sudo`来解决问题，但是要注意不要使用下面的命令：

```shell
$ sudo npm install -g <package_name>
```

而应该给sudo命令带上一个`-E`选项：

```shell
$ sudo -E npm install -g <package_name>
```

### sudo [-E | --preserve-env]

当因为代理配置遇到权限问题时，应该在`sudo`命令后加上`-E`选项来解决问题。**http_proxy**经常使用当前登陆的用户作为请求的用户，而使用sudo命令时，会使用**root**作为请求用户，这样就会因为用户没有权限或者鉴权失败而拒绝响应请求。

sudo命令的`-E`选项会保留当前的环境变量，即命令以**root**用户的身份去执行，但是环境变量中的用户信息保持不变，仍然是原来保存的登陆用户信息，这样就不会导致用户身份不对而触发权限错误。

## 更新全局包

要更新全局安装的包，可以使用：

```shell
$ npm update -g <package_name>
```

如果要更新所有的全局包，可以使用`npm update -g`命令。对于版本低于2.6.1的npm，推荐使用[npm -ungrade-bleeding](https://gist.github.com/othiym23/4ac31155da23962afd0e)来更新所有的全局安装包。

### See Also

* [npm-outdated](https://ninjiahub.github.io/NPM-CLI-Commands/docs/npm-outdated "npm-outdated")

## 卸载全局包

```shell
$ npm uninstall -g <package_name>
```

### See Also

* [npm-uninstall](https://ninjiahub.github.io/NPM-CLI-Commands/docs/npm-uninstall "npm-uninstall")

## 参考

* [Installing npm packages globally](https://docs.npmjs.com/getting-started/installing-npm-packages-globally)

## 声明

本文部分内容来自网络，如有版权问题请联系作者。

侵删。

内容如有不恰当或错误，敬请指正。

作者邮箱：<web.taox@gmail.com>

## Author Info

* [GitHub](https://github.com/Tao-Quixote)
* Email: <web.taox@gmail.com>
