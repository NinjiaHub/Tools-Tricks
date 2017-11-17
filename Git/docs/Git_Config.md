# Git Config

## Git配置文件的位置

在`Unix-like`操作系统中，git配置文件会存在如下三个位置：

* `/etc/gitconfig`
* `~/.gitconfig`
* `.git/config`

其中，`/etc/gitconfig`是系统级的全局配置文件，`~/.gitconfig`在当前用户的根(家)目录下，而`.git/config`文件在每个具体仓库中。

## Git配置文件优先级

`.git/config` > `~/.gitconfig` > `/etc/gitconfig`

即：Git程序在执行某种操作时，优先使用`.git/config`文件中对应的配置项；如果`.git/config`文件中没有，则使用`~/.gitconfig`文件中的配置项；最后会使用`/etc/gitconfig`文件中的配置项，如果`/etc/gitconfig`文件中也没有需要的配置项，则Git使用默认配置项来执行该操作。

## 命令行设置Git配置项

配置方式：

```shell
$ git config --global user.name 'username'
```

这里的`--global`可使用如下替换：

* `--system` 使用此选项时，会修改`/etc/gitconfig`文件
* `--global` 使用此选项时，会修改`~/.gitconfig`文件
* `--local` 使用此选项时，会修改当前仓库中的`.git/config`文件

## 常用设置项 core.*

```
[core]
	editor = 'vim'
	template = 'path/to/template_file'
	pager = 'less'
	excludesfile = '~/.gitignore_global'
	autocrlf = [<true> | <false> | <input>]
```

### core.autocrlf

用于设置换行符转换。

* 在windows中，将`core.autocrlf`设置为`true`，这样在检出代码时，会将`换行`自动转换为`回车换行`。
* 在`Unix-like`系统中，将`core.autocrlf`设置为`input`，这样会在提交时，将`回车换行`自动转换为`换行`。
* Windows开发可讲`core.autocrlf`设置为false来关闭此功能。

### core.editor

设置Git使用的编辑器，用于创建和编辑提交&标签信息。

### core.template

此项的值设置为系统中某个文件的路径，当使用git进行提交的时候，该文件的内容会作为默认的提交信息。

1、例如你设置了`~/.gitmessage-template.txt`文件内容如下：

```
subject line

what happend

[ticket: x]
```
2、然后设置`core.template`

> `$ git config --global core.template '~/.gitmessage-template.txt'`
> 
> `$ git commit`

3、此时，git会使用`core.editor`中设置的编辑器(或者默认编辑器)编辑提交信息，内容如下：

```
subject line

what happened

[ticket: X]
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
# modified:   lib/test.rb
#
~
~
".git/COMMIT_EDITMSG" 14L, 297C
```

可以看到，编辑器顶部的默认信息即为`~/.gitmessage-template.txt`文件中的内容。

**如果你的团队对提交信息有格式要求，可以在系统上创建一个文件，并配置 Git 把它作为默认的模板，这样可以更加容易地使提交信息遵循格式。**

### core.pager

此配置项用于配置Git使用`less``more`等分页器来查看`git log`/`git show`等输出的信息。

### core. excludesfile

在项目中可以使用`.gitignore`文件来设置具体仓库中要忽略的文件，如果要在全局某一类文件，如Mac OS系统中的`.DS_Store`，或者vim编辑器生成的以`~`开头的临时文件，可以使用`core.excludesfile`配置项来实现。

```
# ~/.gitignore_global

*~
.DS_Store
```

```shell
$ git config --global core.excludesfile '~/.gitignore_global'
```

## 常用设置项 user.*

```
[user]
	name = ''
	email = ''
	signingkey = ''
```

### user.name

Git操作中的身份标示。

### user.email

身份标示对应的联系邮箱。因所有人可见，所以最好设置工作邮箱。

### user.signingkey

用于设置`GPG`签署密钥，这样在使用签署提交／tag的时候，不必手动输入密钥。

> `git config --global user.signingkey <gpg-publick-key-id>`
> 
> `git tag -s v0.0.1 -m 'description of tag'`

**对于[提交／tag签署](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E7%AD%BE%E7%BD%B2%E5%B7%A5%E4%BD%9C)不熟悉的可戳。**

## 常用设置项 help.*

```shell
[help]
	autocorrect = 10
```

### help.autocorrect

接受一个代表N个十分之一秒的整数N，Git在你输入错误的Git命令`N/10`秒之后执行它猜测的你想要执行的命令。

例如当我们输入`git cofnig`时，输出：

```shell
git: 'cofnig' is not a git command. See 'git --help'.

Did you mean this?
	config
```

如果我们设置了`help.autocorrect`:

```shell
$ git config --global help.autocorrect 10
```

当我们再次输入错误时，输出如下：

```shell
$ git cofnig

WARNING: You called a Git command named 'cofnig', which does not exist.
Continuing under the assumption that you meant 'config'
in 1.0 seconds automatically...
usage: git config [<options>]

Config file location
    --global              use global config file
    --system              use system config file
    --local               use repository config file

	...

    --show-origin         show origin of config (file, standard input, blob, command line)

```

## 常用设置项 color.*

```
[color]
	ui = [<false> | <true> | <always>]
```

### color.ui

用于设置是否彩色输出。

另：要想具体到哪些命令输出需要被着色以及怎样着色，你需要用到和具体命令有关的颜色配置选项。 它们都能被置为 true、false 或 always：

```
color.branch
color.diff
color.interactive
color.status
```

### color.*

以上每个配置项都有子选项，它们可以被用来覆盖其父设置，以达到为输出的各个部分着色的目的。 例如，为了让 diff 的输出信息以蓝色前景、黑色背景和粗体显示，你可以运行:

```
$ git config --global color.diff.meta "blue black bold"
```

你能设置的颜色有：normal、black、red、green、yellow、blue、magenta、cyan 或 white。 正如以上例子设置的粗体属性，想要设置字体属性的话，可以选择包括：bold、dim、ul（下划线）、blink、reverse（交换前景色和背景色）。

## 声明：

本文中常用设置项部分参考自[自定义git配置](https://git-scm.com/book/zh/v2/%E8%87%AA%E5%AE%9A%E4%B9%89-Git-%E9%85%8D%E7%BD%AE-Git)。

如有版权问题请联系作者，邮箱：web.taox@gmail.com