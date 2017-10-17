# npm更新／卸载本地npm包

## 更新本地npm包

为了获取npm包作者提交的新功能，我们经常需要更新本地项目依赖的npm包。我们可以通过在`package.json`文件所在的目录运行`npm update`命令来更新项目中依赖的npm包。

如何测试`npm update`命令是否运行成功呢？可以通过运行`npm ourdated`来测试是否全部依赖更新完成。如果运行没有输出，表示`npm update`命令已经更新了所有本地依赖的npm包。

### See Also

* [npm-update](https://github.com/NinjiaHub/NPM-CLI-Commands/blob/master/documents/npm-update.md)
* [npm-outdate](https://github.com/NinjiaHub/NPM-CLI-Commands/blob/master/documents/npm-outdate.md)

## 卸载本地npm包

如果项目中不再依赖某个npm包，可以通过下面的命令从本地移除：

```shell
$ npm uninstall <package_name>
```

上面的命令只会从`node_modules`目录中将包移除掉，但是并没有从`package.json`文件中删除该包的安装信息，所以如果如果下次在`package.json`文件所在的目录中运行`npm install`，还是会安装已经移除的包。这不是我们想要的，所以对于项目不再依赖的npm包，应该从`package.json`文件中删除该包的安装信息：

```shell
$ npm uninstall --save <package_name>
or
$ npm uninstall --save-dev <package_name>
```

`--save`只会移除保存在`package.json`文件中`dependencies`部分中的安装包信息，而`--save-dev`只会移除保存在`devDependencies`部分中的安装包信息。

### See Also

* [npm-uninstall](https://github.com/NinjiaHub/NPM-CLI-Commands/blob/master/documents/npm-uninstall.md)

## 参考

* [Updating local packages](https://docs.npmjs.com/getting-started/updating-local-packages)
* [Uninstalling local packages](https://docs.npmjs.com/getting-started/uninstalling-local-packages)

## 声明

本文部分内容来自网络，如有版权问题请联系作者。

侵删。

内容如有不恰当或错误，敬请指正。

作者邮箱：web.taox@gmail.com。

## Author Info

* [GitHub](https://github.com/Tao-Quixote)
* Email: web.taox@gmail.com