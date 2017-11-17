# Git配置GPG密钥及签名tag & 签名commit

## GPG入门教程

不知道如何使用GPG的，可以查看这篇阮一峰老师的[《GPG入门教程》](http://www.ruanyifeng.com/blog/2013/07/gpg.html)。

## GPG key id

由于GPG生成的公钥特别长，因此GPG根据公钥的内容生成了`指纹(fingerprint)`，这样就可以使用更短的hash值来表示本地唯一的公钥。在书写的时候更方便。

fingerprint：

```shell
Fingerprint: 0D69 E11F 12BD BA07 7B37  26AB 4E1F 799A A4FF 2279
Long key ID:                                4E1F 799A A4FF 2279
Short key ID:                                         A4FF 2279
```

从上面我们可以看出完整的`fingerprint`长40个字节。为了方便书写，GPG还支持`Long key ID`和`Short key ID`两种写法，鉴于我们本地不会生成特别多的GPG密钥，所以在平时的使用中，我们可以使用`Short key ID`来表示一个特定的公钥。

## 配置Git中的user.signingkey

不熟悉Git配置的同学可以👉[戳这里(Git Config)](http://tools-tricks.taojihede.com/Git/docs/Git_Config "git config")。

`user.signingkey`的作用是设置一个git默认使用的GPG公钥对应的`key id`，这样当使用Git的`签名tag`和`签名commit`功能时，git会根据`user.signingkey`中设置的`key id`获取对应的GPG公钥来签署`tag`或者`commit`。

可以通过下面的命令来配置`user.signingkey`：

```shell
$ git config --global user.signingkey A4FF 2279
```

通过上面的配置项，在签署tag或者commit的时候，就不需要手动指定`key id`了。

## GPG签署tag

由于我们上面设置了`user.signingkey`，所以可通过下面这种比较简单的方式签署一个tag：

```shell
$ git tag -s <tagname> -m "some info about this tag."
```

如果我们没有设置`user.signingkey`，也可以显示地声明`<keyid>`来使用指定的GPG公钥来签署一个tag：

```shell
$ git tag <tagname> -u <keyid> -m "some info about this tag."
```

**Git tag相关的操作请👉[戳这里(Git中的tag)](http://tools-tricks.taojihede.com/Git/docs/Git_tag "git tag")**

## GPG签署commit

使用GPG签署提交的方式很简单，只需要在提及命令中添加一个`-S[keyid]`选项，如果设置了`user.signingkey`，则`keyid`可不写，使用默认GPG密钥：

```shell
$ git commit -S -m "info about the commit"
```

## 显示签名

`git show`以及`git log`命令有一个共同的选项`--show-signature`，可用来查看tag和commit的签名。

示例：

```shell
$ git show --show-signature HEAD
commit e333887b93aa7db85eb13f013b8aa71973518fdf
gpg: Signature made Fri Sep 29 21:26:10 2017 CST
gpg:                using RSA key 175347FDBA20A2487A50CF979B3D58282B02B4A6
gpg: Good signature from "taoxin <web.taox@gmail.com>" [ultimate]
Author: TaoQuixote <web.taox@gmail.com>
Date:   Fri Sep 29 21:26:10 2017 +0800

    sign a commit

diff --git a/.gitmodules b/.gitmodules
new file mode 100644
index 0000000..5e56f6b
--- /dev/null
+++ b/.gitmodules
@@ -0,0 +1,3 @@
+[submodule "Tools-Tricks"]
+       path = Tools-Tricks
+       url = git@github.com:NinjiaHub/Tools-Tricks.git
diff --git a/Tools-Tricks b/Tools-Tricks
new file mode 160000
index 0000000..e8ae2d7
--- /dev/null
+++ b/Tools-Tricks
@@ -0,0 +1 @@
+Subproject commit e8ae2d79a5c95550a69efb1a479d40a6224125b1
```

另，`git tag`命令有一个`-v`选项，可用于验证一个tag的GPG签名：

```shell
$ git tag -v v0.0.1

object f7b607eedd66c0fb07b93604cf5446a11cdca2b6
type commit
tag v0.0.1
tagger TaoQuixote <web.taox@gmail.com> 1506605297 +0800

Add v0.0.1
gpg: Signature made Thu Sep 28 21:28:31 2017 CST
gpg:                using RSA key 175347FDBA20A2487A50CF979B3D58282B02B4A6
gpg: Good signature from "taoxin <web.taox@gmail.com>" [ultimate]
```

## 验证签名后合并/拉取

在平时的开发工作中，我们可以通过验证公钥在自己钥匙链(key chain)中的GPG签名来合并可信任的提交，这样可以保证我们合并/拉取的代码是安全、干净的。

git 1.8.3及之后的版本支持在`git pull`和`git merge`中使用`--verify-signature`选项来验证要合并/拉取的提交的作者的公钥是否在自己的钥匙链中，如果作者的公钥在自己的钥匙链中，可认为这个提交是安全可信任的；如果作者的公钥不在自己的钥匙链中，则拒绝合并/拉取：

```shell
$ git merge --verify-signatures <commit>
```

如果`commit`作者的公钥不在自己的钥匙链中，则合并会自动中断。

## 是否验证GPG签名

验证GPG签名可以保证我们获取的commit都是可信赖的。

我们可以在一个开源项目中使用验证GPG签名的流程，这样就可以减少不必要的麻烦(例如恶意提交无用代码)；但是这也会给初次提交`pull request`的贡献者造成一定的困扰，例如初次贡献者的公钥不在代码管理者的钥匙链中。

如果我们在公司内部的开发中使用验证GPG签名的流程，要确保参与开发的同事都了解如何使用GPG签名，不要因为使用GPG签名而带来不必要的麻烦，如果很多同事都不知道如何使用GPG来签署提交，这会花费你很多时间来帮助他们给他们的提交添加签名。

虽然Git是通过密码来保证身份的，但是加上GPG签名可以让代码管理流程更安全。所以如果团队中的成员都了解如何使用GPG来签署提交的话，建议在Git工作流中添加GPG签名验证。

## 声明
1、该文章部分内容摘抄自[Git 工具 - 签署工作](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E7%AD%BE%E7%BD%B2%E5%B7%A5%E4%BD%9C),如有版权问题请联系作者。

2、本文在第一部分：GPG入门教程部分中引用了**阮一峰老师**的[《GPG入门教程》](http://www.ruanyifeng.com/blog/2013/07/gpg.html)，如涉及版权问题请联系作者。

侵删。

邮箱：web.taox@gmail.com