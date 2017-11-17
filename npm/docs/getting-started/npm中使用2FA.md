# npm中使用2FA

2FA：Two-Factor Authentication(双因子验证)，是一种相对于传统密码来说更为安全的密码验证方式。传统密码是由一组静态信息组成：数字、字母、手势、图片等，比较容易被捕获；而2FA是在传统密码验证之后的另外一层验证，即双层验证，而且有别于传统密码，2FA是基于时间、历史长度等一系列自然事物结合一定的加密算法算出来的一组动态的密码，而且该密码经过一定的时间就会重新生成，不容易被捕获，所以相对于传统密码更加安全。

2FA最常见的使用场景即我们在购物消费时银行的短信验证码，在我们输入了正确的用户名和密码之后，银行会要求第二层动态验证，一旦有任何不对劲的地方，银行系统就会中止交易；只有在静态密码和动态验证信息都输入正确验证无误的情况下，交易才能顺利进行。

动态验证提供了额外的安全保护，如果别人没有你的产生/接收动态密码的设备，即使别人获取了你的用户名和密码，也不能登录你的账户。

npm为了保护用户账户安全，避免未授权的登录用户账户，也引入了2FA。

## 准备

为了开启npm账户的2FA功能，首先需要一个可以产生OTP(One Time Password)的APP。例如[Authy](https://authy.com/download/)和[Google身份验证器](https://support.google.com/accounts/answer/1066447)，可以用来产生OTP。这两个应用使用TOTP(Time-Baseed One-Time Password)算法来生成临时码。请在移动设备或者电脑上安装这两个应用，以便于在使用npm账户时可以使用其生成的临时码。

**注：npm不使用SMS作为动态验证。**

## 验证等级

npm中对于2FA有两种等级：***auth-only*** 和 ***auth-and-write***。

不同等级模式包含不同需要2FA的操作，***auth-only***在执行下面的操作时需要OTP：

* 登录(login)
* 移除2FA

<span id="auth-and-write">***auth-and-write***</span>模式在执行以下操作时需要OTP：

* 登录(login)
* 更改个人简介信息(profile)
* 新建/移除token
* 发布npm包
* 修改权限(access)
* 重置密码
* 对npm包执行其他敏感操作时
* 移除2FA

如果要在命令行中带上**OTP**，可以使用如下方式：

```shell
$ npm owner add <user> --otp=123456
```

其中，`123456`为你设备上当前的OTP。

## <span id="enable-2fa">如何开启2FA</span>

为了开启2FA，可以使用如下命令：

```shell
$ npm profile enable-2fa
```

npm中，如果不明确模式，则默认使用[***auth-and-write***](#auth-and-write)模式；也可以通过下面的命令明确指出使用哪种模式：

```shell
$ npm profile enable-2fa auth-and-write
> npm notice profile Enabling two factor authentication for auth-and-writes   

$ npm profile enable-2fa auth-only
> npm notice profile Enabling two factor authentication for auth-only
```

在输入上面的命令之后，npm可能会提示你输入用户名和密码，在正确的输入之后，npm会在命令行显示一个二维码，这个二维码中包含要给身份验证器使用的序列号用以绑定你的npm账户；可以使用上面安装好的app扫描这个二维码，身份验证器会自动将你的npm账户和身份验证绑定。绑定之后身份验证器会给出一个有时效性地临时码，在npm提示的位置输入这个临时码，就完成了npm中账户与身份验证器的绑定。在后续的使用中，如果使用的npm命令需要验证2FA中的临时码，打开安装好的app查看、输入即可。

通常在二维码的下方会有如下的提示：

```shell
Add an OTP code from your authenticator:
```

输入身份验证器中的OTP之后，命令行会输出如下提示信息：

```shell
2FA successfully enabled. 
Below are your recovery codes, please print these out. 
You will need these to recover access to your account 
if you lose your authentication device.
```

这行提示在`5.5.1`版本中会以5个**恢复码(recovery codes)**作为结束，请在安全的地方保存好这5个恢复码。如果安装身份验证器的设备丢失了，可以通过恢复码解绑原来绑定的2FA设备，然后设置新的2FA设备。

**注：除了恢复码之外，有的身份验证器app会提供别的办法用来帮助存储恢复码。**

### 从个人设置中移除2FA

要从个人设置中移除2FA，可以使用`npm profile`命令中的`disable-2fa`选项：

```shell
$ npm profile disable-2fa
```

npm会提示输入密码以确认身份：

```shell
> npm password:
```

然后会提示输入OTP(One Time Password)做二次确认：

```shell
>Enter one-time password from your authenticator:
```

如果输入信息正确，npm会提示移除2FA成功的信息：

```shell
Two factor authentication disabled. 
```

### 在命令行中带上OTP

如果开启了2FA中的***auth-and-writes***模式，对于特定的npm命令，在使用时需要发送OTP到服务器。可以在命令行的末尾加上`--otp=123456`，其中，**123456**为OTP。下面是几个例子：

```shell
npm publish [<tarball>|<folder>][--tag <tag>] --otp=123456
npm owner add <user > --otp=123456
npm owner rm <user> --otp=123456
npm dist-tags add <pkg>@<version> [<tag>] --otp=123456
npm access edit [<package>) --otp=123456
npm unpublish [<@scope>/]<pkg>[@<version>] --otp=123456
```

### 如果安装身份验证器的设备丢了

* 1、找到前面保存的恢复码(recovery codes)
* 2、如果退出登录，使用用户名和密码登录；当命令行提示输入OTP时，输入一个恢复码(recovery codes)
* 3、登录成功之后，使用`npm profile disable-2fa`命令移除2FA；如果提示输入密码则再次输入密码
* 4、此时npm命令行会提示你输入OTP确认身份，因为设备丢了没有OTP，所以当命令行提示输入OTP时，输入一个未使用过的恢复码
* 5、npm会提示已经移除Two-Factor Authentication(2FA)
* 6、如果要再次使用2FA，则在另外一台设备上安装好身份验证器后，重复[如何开启2FA](#enable-2fa)中的操作

**如果连恢复码也找不到了，请联系npm客户支持。**

### Note

在命令中的设置会同步到[NPM官网](npmjs.com)，因此，只需要在命令行中激活2FA就可以了。

## 参考

* [How to enable 2FA for NPM](https://authy.com/guides/npm/)
* [Using dist-tags](https://docs.npmjs.com/getting-started/using-tags)

## 声明

本文部分内容来自网络，如有版权问题请联系作者。

侵删。

内容如有不恰当或错误，敬请指正。

作者邮箱：web.taox@gmail.com。

## Author Info

* [GitHub](https://github.com/Tao-Quixote)
* Email: web.taox@gmail.com
