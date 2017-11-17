# Git init

当需要一个新的仓库的时候，可以使用两个命令: `git init` 和 `git clone`，这两个命令很容易搞混，因为在高层来看，这两个命令可以用来创建新的仓库，但是`git clone`对`git init`是有依赖的，所以追根究底，git仓库还是通过`git init`来创建的。

`git clone`只能用来clone一个已经存在的本地或者远程仓库。`git clone`首先会调用`git init`命令来创建一个空的仓库，然后将远程仓库中的数据拷贝到新建的仓库中，最后捡出一份快照放在工作目录中。

## 用法

可以通过命令`man git-init`来看一下`git init`命令的使用方法概述：

```shell
git init [-q | --quiet] [--bare] [--template=<template_directory>]
	  [--separate-git-dir <git dir>]
	  [--shared[=<permissions>]] [directory]
```

最基本的创建仓库的办法就是进入要初始化为git仓库的目录，然后执行`git init`：

```shell
$ cd path/to/repository
$ git init
```
这会将repository目录转化为git仓库。

还可以通过指定目录的方式来初始化git仓库：

```shell
$ git init path/to/git-repository
```
这会将指定的目录转化为git仓库。

### git仓库依赖的存放位置

当使用`git init [directory]`来初始化一个git仓库时，如果没有特殊设置，git会将该仓库依赖的所有文件放在`.git`目录中，并且将所有的快照文件放在`.git/objects`目录中，这是git的默认行为。如果我们要自己指定git仓库依赖的目录和快照存放的目录的话，可以通过配置`$GIT_DIR` 和 `$GIT_OBJECT_DIRECTORY`来实现。

如果环境变量中没有设置`$GIT_DIR`，git默认将当前仓库的所有依赖放在`.git`目录中，如果我们设置`$GIT_DIR`为`.repo`，git在初始化仓库的时候就会将所有依赖放在`.repo`目录中。首先，不推荐修改`$GIT_DIR`，一是因为修改这个没有太大的必要，二是可能会给熟悉git的人造成一定的疑惑(因为依赖通常放在`.git`目录中)；其次，如果真的要修改`$GIT_DIR`，建议使用`.dir_name`，即使用隐藏目录作为依赖的存放目录。

如果没有设置`$GIT_OBJECT_DIRECTORY`变量，git在初始化仓库时会将快照文件(blob)存放在`$GIT_DIR/objects`目录中，所以如果想要修改快照文件的存放位置，可以通过设置`$GIT_OBJECT_DIRECTORY`变量来实现。同样，不推荐修改快照文件的存放位置。

### 新建git仓库的结构

假设我们指定新的git仓库在`my-repo`目录中，当我们在该目录中创建新的仓库的时候：

```shell
$ cd my-repo
$ git init
```

`my-repo`目录的结构如下：

```git
# my-repo

|- .git
	|- HEAD
	|- config
	|- objects
		|- info
		|- pack
	|- refs
		|- heads
		|- tags
```

当我们把一个目录初始化为一个git仓库时，git根据`$GIT_DIR` 和 `$GIT_OBJECT_DIRECTORY`，将git仓库用到的所有依赖放在`.git`目录中。

## git init的可选参数

### --quiet

当使用`--quiet`选项时，git在初始化仓库的过程中只会输出错误信息和警告信息，其他信息都会被忽略掉。

### --bare

当使用`--bare`选项时，git会创建一个“裸”库。“裸”库不是指什么都没有，而是指git仓库的依赖、配置等文件裸露在仓库中，没有使用`.git`这种目录隐藏依赖和配置等文件。

当使用`--bare`选项创建仓库时，仓库结构如下：

```git
# my-repo

|- HEAD
|- branches
|- config
|- description
|- hooks
|- info
|- objects
	|- info
	|- pack
|- refs
	|- heads
	|- tags
```

所有的配置、依赖文件裸露在仓库中，没有使用`.git`目录来隐藏，而且没有工作目录。

`--bare`仓库是用来创建中央仓库，这这种仓库的特点是没有工作目录，所以通过`--bare`创建的仓库不建议进行编辑操作，只允许`pull` 和 `push`操作。所以可以使用该命令在自己的服务器上搭建一个git仓库作为中央仓库进行代码管理。

### --template=\<template_directory\>

`--template`选项用来指定git仓库的**模版**，这里的模版是指git仓库依赖、配置的模版，而不是工作目录的模版。

当我们执行下面的命令时：

```shell
$ git init --template=absolute/path/to/template repo_name
```

git会将template目录下的所有文件拷贝到`$GIT_DIR`指定的目录中，如果`$GIT_DIR`没有设置则拷贝到默认的`.git`目录中。

新建git仓库`.git`目录结构：

```git
# my-repo

|- .git
	|- HEAD
	|- config
	|- objects
		|- info
		|- pack
	|- refs
		|- heads
		|- tags
```

如果模版所在目录中包含上面`.git`目录中这些文件，git则直接将模版目录下的文件拷贝到`$GIT_DIR`指定的目录中；如果缺少git仓库依赖的配置、存储等文件或者目录时，git会自己创建一个默认的来代替模版目录中缺失的部分。

`$GIT_DIR`目录中文件的来源顺序如下：

* 如果命令行使用了`--template`选项，则使用`--template`之后的路径中的文件填充`$GIT_DIR`
* 如果设置了`$GIT_TEMPLATE_DIR`环境变量，则使用`$GIT_TEMPLATE_DIR`指定的路径中的文件填充`$GIT_DIR`
* 如果配置了`init.temlateDir`，则使用其指定的目录
* 默认的模版目录：`/usr/share/git-core/template`

**注：模版文件所在的目录，要使用绝对路径来指定。**

### --separate-git-dir=\<git dir\>

当创建仓库时使用这个选项，git依赖的文件既不会放在`$GIT_DIR`指定的目录中，也不会放在`.git`目录中，而是将git仓库的所有依赖存放到`<git dir>`指定的目录中。

```shell
$ git init --separate-git-dir=/Users/taoxin/workspace/git/git_repo_dir my_repo

$ cd my_repo
$ ls -Al
-rw-r--r--  1 taoxin  staff  49 Oct 13 18:20 .git

$ cat .git
gitdir: /Users/taoxin/workspace/git/git_repo_dir
```

从上面的代码可以看出，当使用`--separate-git-dir`选项时，git并没有创建存放仓库依赖的目录，而是创建了一个文本文件，其中存放着真正存放git仓库依赖的目录的路径。

```shell
$ ls git_repo_dir
HEAD         branches/    config       description  hooks/       info/        objects/     refs/
```

真正的git仓库依赖存放在`git_repo_dir`目录中。

## 参考链接

* [git init](https://www.atlassian.com/git/tutorials/setting-up-a-repository/git-init)
* [git-init - Create an empty Git repository or reinitialize an existing one](https://git-scm.com/docs/git-init)

## 声明

本文部分内容参考自网络，如有版权问题请联系作者。

侵删。

如内容有错误，敬请指正。

作者邮箱：web.taox@gmail.com。