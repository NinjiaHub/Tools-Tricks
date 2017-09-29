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

不熟悉Git配置的同学可以👉[戳这里(Git Config)](https://github.com/NinjiaHub/Tools-Tricks/blob/master/documents/Git/Git_Config.md)。

`user.signingkey`的作用是设置一个git默认使用的GPG公钥对应的`key id`，这样当使用Git的`签名tag`和`签名commit`功能时，git会根据`user.signingkey`中设置的`key id`获取对应的GPG公钥来签署`tag`或者`commit`。

可以通过下面的命令来配置`user.signingkey`：

```shell
$ git config --global user.signingkey A4FF 2279
```

通过上面的配置项，在签署tag或者commit的时候，就不需要手动指定`key id`了。

## GPG签署tag

## GPG签署commit

## 声明
1、该文章部分内容摘抄自[Git 工具 - 签署工作](https://git-scm.com/book/zh/v2/Git-%E5%B7%A5%E5%85%B7-%E7%AD%BE%E7%BD%B2%E5%B7%A5%E4%BD%9C),如有版权问题请联系作者。

2、本文在第一部分：GPG入门教程部分中引用了**阮一峰老师**的[《GPG入门教程》](http://www.ruanyifeng.com/blog/2013/07/gpg.html)，如涉及版权问题请联系作者。

侵删。

邮箱：web.taox@gmail.com