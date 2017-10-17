# Git中的tag

在Git中，我们可以通过打标签的方式来表示某次提交的重要性。在开源社区中，经常使用`tag`来表示版本号，以表示这次提交是一个比较重要的节点。

## tag的种类

tag分为两种：`轻量标签(lightweight)`和`附注标签(annotate)`。而如果继续细分的话，附注标签又可以分为`签名标签`和`未签名标签`。

**注：Linux环境下，Git提供了完善的帮助文档，当不知道一个git命令如何使用时，可用`git command --help`来查看帮助文档，这里的`command`指具体的命令。**

### 1、附注标签(annotate)

> If one of **`-a`**, **`-s`**, or **`-u <keyid>`** is passed, the command creates a tag object, and requires a tag message. Unless **`-m <msg>`** or **`-F <file>`** is given, an editor is started for the user to type in the tag message.

> If **`-m <msg>`** or **`-F <file>`** is given and **`-a`**, **`-s`**, and **`-u <keyid>`** are absent, **`-a`** is implied.

> Tag objects (created with **`-a`**, **`-s`**, or **`-u <keyid>`**) are called "annotated" tags; they contain a creation date, the tagger name and e-mail, a tagging message, and an optional GnuPG signature.
```

从帮助文档可以看出，当传递了**`-a`**, **`-s`**, or **`-u <keyid>`**中的任意一个可选参数时，git会创建一个`标签对象(tag object)`；tag的创建同`commit`一样需要填写说明信息，如果没有使用**`-m <msg>`** 或者 **`-F <file>`**的话，git会打开一个默认文本编辑器来编辑说明信息。

如果传递了**`-m <msg>`**或者**`-F <file>`**给git，但是没有传递**`-a`**, **`-s`**和**`-u <keyid>`**中的任何一个时，git默认使用**`-a`**来创建一个`未签名的附注标签`。

git生成的附注标签对象中包含创建时间、tag名字、作者的email地址、标签说明信息和可选的`GPG`签名信息。

```shell
$ git tag -a v0.0.1 -m "annotate whiuout sign"    // 生成不带签名的tag
$ git tag -s v0.0.1 -m "annotate whith sign"      // 生成带签名的tag
```

下面是一个带有`GPG`签名的`tag`的详细信息：

```shell
$ git show v0.0.1

tag v0.0.1
Tagger: TaoQuixote <web.taox@gmail.com>
Date:   Thu Sep 28 21:28:17 2017 +0800

Add v0.0.1
-----BEGIN PGP SIGNATURE-----

iQIzBAABCAAdFiEEF1NH/bogokh6UM+Xmz1YKCsCtKYFAlnM+P8ACgkQmz1YKCsC
tKY/Dw//S7CPuBFcPeV6eK55u5R7mUfoKpl4OkXNwxUOp31Hul5Dm1vsVC9Q/PPH
tpB0g3bYA72/3GvrjTARKPOyhIjeMZJRU8hnrTKOEFUO1xJ8rm+sPIn+IGf+hsjh
c8sbfiwdh18a/z2eA0rI/wrn5YpmZHCKBQwOMjqN1klhQH6n4KVPYLhaWOoT1x+R
slIO3N+CIgVMv5ifhdl1QxxS4ZXCQYQyFLc70R1zEuobT8UQKSHoP40My6dLVI4i
y+ZB2ZKe7keHyNgP0RAfJaqclmqNLM2lAbX+NuCAmn3a00ULmvjmZVnHDtPiCuQZ
DFQP69STisfu81KKX4j0MYFTmn5/GVTaaYCUgxpqRosIh5oaZs2baVmz96gxbiRS
ZWyRY4wTtfBillZaWonawYqVaiGDdO0Os+VHjZ3E+1Tac3MoaneQ9CDPZLyKgOKu
Hpj5IkToic1VCt3Mz15JxmavI8c4aH67/Im64Nyvq7H24Z0q8AWYWiWDkQdgK5ne
YZ4J4Mdd7RBrKThb6KTunFfYJudaQKn7jx8EpoHQk6joBx0Ip9Ys1Q6jqf1GFMiN
hbEK7ow+bQconzL8YBZrJUVZMaKza/z+sribK6kYX8dWd15OAsCaVsdpYotqwOh4
JsED1r13QiK4p1awJ0wA2LYkSeJVClJHfRY08+Vo9rHbOiSdweU=
=TEfX
-----END PGP SIGNATURE-----

commit f7b607eedd66c0fb07b93604cf5446a11cdca2b6
Author: TaoQuixote <web.taox@gmail.com>
Date:   Thu Sep 28 16:42:17 2017 +0800

    version 2 on featurea

diff --git a/a.txt b/a.txt
index 23a266e..a3285e4 100644
--- a/a.txt
+++ b/a.txt
@@ -1,2 +1,4 @@
 
 version 1 on master
+
+version 2 on featurea
```

### 2、轻量标签(lightweight)

`轻量标签(lightweight)`可以看作一个`commit`的`软连接`，即单纯地指向这个提交，并不会生成一个标签对象来表示这个标签；由于轻量标签只是单纯地指向一个提交对象，所以当使用`git show tagname`来查看轻量标签的信息，只能看到该标签指向的提交对象所包含的信息。

**建议在使用git的tag的功能时，使用`附注标签`来表示一次比较重要的发版，而`轻量标签`只是用来表示私人对一次提交的标注。**

### 3、GPG

Git中配置`GPG`密钥及设置`tag``commit`签名请[👇戳这里](https://github.com/NinjiaHub/Tools-Tricks/blob/master/Git/documents/Git%E9%85%8D%E7%BD%AEGPG%E5%AF%86%E9%92%A5%E5%8F%8A%E7%AD%BE%E5%90%8Dtag%E5%92%8C%E7%AD%BE%E5%90%8Dcommit.md)

## 补tag

在开发过程中可能忘记打标签，Git允许对之前的提交补tag。

首先我们通过`git log`命令查看之前的提交记录：

```shell
$ git log --oneline --graph

* c1ae8a1 Revert "Revert "merge from featurea to master""
* 2fcfe24 Revert "merge from featurea to master"
*   fc6b0bc merge from featurea to master
|\  
| * c221063 version 3 on featurea
| * f7b607e version 2 on featurea
* | 4ba4ad6 version 3 on master
* | 0284cb7 version 2 on master
|/  
* 7ed61cb version 1
* 3f7acd5 + init repo
```

然后，找到我们要打标签的提交对应的SHA-1值，例如`4ba4ad6 `，执行：

```shell
$git tag
v0.0.1
v0.0.2

$ git tag -a v0.0.3 4ba4ad6 -m "annotate v0.0.3"
$ git tag

v0.0.1
v0.0.2
v0.0.3
```

然后我们可以看到，标签`v0.0.3`生成了。

## 列出标签

可以通过如下命令展示当前仓库中的所有标签：

```shell
$ git tag
v0.0.1
v0.0.2
```

## 将标签推送到远程仓库

`git push`命令默认不会推送`tag`到远程仓库，所以需要显式地手动推送：

推送单个`tag`到远程仓库：

```shell
$ git push [remote_name] [tagname]
```

推送本地所有`tag`到远程仓库：

```shell
$ git push --tags
```
这会将本地所有不在远程仓库中的`tag`推送到远程仓库中。

## 检出标签

我们可以通过如下命令检出特定标签：

```shell
$ git checkout v0.0.1
```

或者可以通过下面的命令在特定标签的基础上新建一个分支：

```
$ git checkout -b [branchname] [tagname]
```

```shell
$ git checkout -b version0.0.2 v0.0.1

Previous HEAD position was 2fcfe24... Revert "merge from featurea to master"
Switched to a new branch 'version0.0.2'
```

可以看到git在标签`v0.0.1`的基础上创建并切换到了一个新的分支`version0.0.2`

## 声明
该文章部分内容摘抄自[Git 基础 - 打标签](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%89%93%E6%A0%87%E7%AD%BE),如有版权问题请联系作者。

侵删。

邮箱：web.taox@gmail.com