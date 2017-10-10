# Git忽略文件

在项目开发过程中，有些文件我们不想放在git中管理，例如：

* 依赖缓存：`/node_modules`、`/packages`
* 编译过程中生成的文件：`.o`、`.class`等
* 构建时的输出目录：`/build`、`/out`、`/bin`、`target`等
* 运行时文件：`.log`、·`ock` 或者 `.tmp`
* 隐藏的系统文件：`.DS_Store` 或者 `Thumbs.db`
* 个人的IDE配置文件，如`.idea/workspace.xml`

以上这些文件，没有必要通过git进行版本管理，而且有的甚至会引起不必要的麻烦。假如我们不小心将一个系统文件传到了远程git服务器中，另外一个同事更新代码后，本地的系统文件被覆盖掉了，可能会修改同事的设置，甚至破坏同事的系统等。

## .gitignore

`gitignore`文件用来指定git要忽略的文件，`gitignore`文件的目的是用来保证git不会跟踪未跟踪状态的文件，所以`gitignore`文件对于已经处于跟踪状态的文件是无效的。如果想要停止跟踪已经处于跟踪状态的文件，可以使用如下命令：

```shell
$ git rm [--cached] <file>		// 移除文件
$ git rm [-r] [--cached] <file>	// 递归移除目录
```

注：上面命令中的`--cached`是可选项，当使用该选项时，git只会从仓库中移除该文件/目录；如果不使用该选项，git会从仓库和本地文件系统中移除该文件/目录，请谨慎使用，因为移除的文件会被彻底删除，你的工作可能会丢失。

`gitignore`文件中的每一行声明一个匹配模式，当git决定是否要忽略一个路径/文件时，会从下面列出的三种设置gitignore的文件以及命令行中获取匹配模式，如果当前路径/文件与匹配模式匹配，则忽略该文件，不加入版本管理。

## .git/info/exclude

`.git/info/exclude`和`.gitignore`设置方式一样，区别是`.git/info/exclude`文件只存在本地仓库中的，git不会将其加入版本管理。所以`.git/info/exclude`适合放置私人针对单一仓库的配置。

## 全局gitignore

`.gitignore`文件的作用域仅限于其所在的目录，git不仅支持局部gitignore，而且支持全局gitignore，设置方式如下：

```shell
$ touch ~/.gitignore
$ git config --global core.excludesFile ~/.gitignore
```

上面的命令中，第一行中的`.gitignore`文件可以放在有权限的任意目录中，但是推荐放置在家目录中；第二行命令用来设置git去哪里获取该gitignore文件。

## gitignore的优先级

在三种gitignore的设置中，`.gitignore`文件的优先级最高，`.git/info/exclude`次之，`全局gitignore`的优先级最低。

由于可以设置多个`.gitignore`文件，不同目录中不同`.gitignore`文件的优先级也不相同，离的越近优先级越高，如下：

```
|-root
  |- .gitignore1
  |- dir
    |- a.txt
    |- .gitignore2
    
# .gitignore1
a.txt

# .gitignore2
!a.txt
```
> 注：`.gitignore1`和`.gitignore2`是为了方便区分，实际操作中请使用`.gitignore`。

对于目录`dir`中的文件来说，`.gitignore2`的优先级要高于`.gitignore1`；即使我们在`.gitignore1`中忽略了`a.txt`，但是`.gitignore2`中设置不要忽略`a.txt`，所以最终git仍会将`a.txt`纳入版本管理。

## 偷懒(工具)

不得不说，偷懒才是第一生产力，程序员群体尤其如此，为了偷懒，我们发明了一个又一个工具😊。

下面两个链接中：第一个指向github上的[`gitignore`](https://github.com/github/gitignore)仓库，该仓库中有很多开发语言可以使用的`.gitignore`模版；第二个指向[gitignore.io](https://www.gitignore.io/)，该站点支持输入`操作系统``IDE`和`编程语言`来生成一个通用的`.gitignore`文件。

这两种模版的使用都比较简单，这里就不做说明了。

* [github/gitignore](https://github.com/github/gitignore)
* [gitignore.io](https://www.gitignore.io/)

## 参考链接

* [git-scm.com/docs/gitignore](https://git-scm.com/docs/gitignore)
* [gitignore](https://www.atlassian.com/git/tutorials/gitignore)
* [Git-基础-记录每次更新到仓库](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E8%AE%B0%E5%BD%95%E6%AF%8F%E6%AC%A1%E6%9B%B4%E6%96%B0%E5%88%B0%E4%BB%93%E5%BA%93)

## 声明

文中部分内容来源于网络，如有版权问题请联系作者。

侵删。

作者邮箱：web.taox@gmail.com