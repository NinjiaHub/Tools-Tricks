# README

主要介绍Node中的js包管理工具：`npm`相关的概念、用法以及一些常见问题的解决办法。

该系列文章的主要参考文档为[npm官方文档](https://docs.npmjs.com/)，其余参考文章及文档，会在文章中的**参考**部分给出。

## 推荐阅读顺序

* 起步
	* 概念
		* [什么是npm](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/%E4%BB%80%E4%B9%88%E6%98%AFnpm.md)
	* 安装 
		* [修复npm安装全局包/命令的权限问题](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/npm%E6%9D%83%E9%99%90%E9%97%AE%E9%A2%98.md)
		* [局部安装npm包](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/npm%E5%B1%80%E9%83%A8%E5%AE%89%E8%A3%85%E5%8C%85%E7%9A%84%E7%AE%A1%E7%90%86.md)
		* [使用package.json](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/%E4%BD%BF%E7%94%A8package.json.md)
		* [npm更新／卸载本地npm包](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/npm%E6%9B%B4%E6%96%B0%E4%B8%8E%E5%8D%B8%E8%BD%BD%E6%9C%AC%E5%9C%B0npm%E5%8C%85.md)
		* [npm全局安装包的管理](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/npm%E5%85%A8%E5%B1%80%E5%AE%89%E8%A3%85%E5%8C%85%E7%9A%84%E7%AE%A1%E7%90%86.md)
	* 创建/发布npm包
		* [创建Node.js模块](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/%E5%88%9B%E5%BB%BANode.js%E6%A8%A1%E5%9D%97.md)
	* 进阶
		* [SemVer](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/SemVer.md)
		* [Node查找依赖包/库的规则](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/Node%E6%9F%A5%E6%89%BE%E4%BE%9D%E8%B5%96%E5%8C%85-%E5%BA%93%E7%9A%84%E8%A7%84%E5%88%99.md)
		* [使用范围包-scoped-packages](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/getting-started/%E4%BD%BF%E7%94%A8%E8%8C%83%E5%9B%B4%E5%8C%85-scoped-packages.md)
		* [使用标签](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/%E4%BD%BF%E7%94%A8%E6%A0%87%E7%AD%BE.md)
		* [npm中使用2FA](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/npm%E4%B8%AD%E4%BD%BF%E7%94%A82FA.md)
		* [使用token](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/%E4%BD%BF%E7%94%A8token.md)
		* [使用命令行修改个人简介](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/getting-started/%E4%BD%BF%E7%94%A8%E5%91%BD%E4%BB%A4%E8%A1%8C%E4%BF%AE%E6%94%B9%E4%B8%AA%E4%BA%BA%E7%AE%80%E4%BB%8B.md)
* How npm works
	* [01-包和模块(Packages-and-Modules)](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/how-npm-works/packages.md)
	* [02-npm v2](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/how-npm-works/npm2.md)
	* [03-npm v3](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/how-npm-works/npm3.md)
	* [04-npm v3 依赖重复](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/how-npm-works/npm3-dupe.md)
	* [05-npm v3 不确定性](https://github.com/NinjiaHub/Tools-Tricks/blob/master/npm/documents/how-npm-works/npm3-nondet.md)
* CLI Commands
	* [CLI Commands Git仓库地址](https://github.com/NinjiaHub/NPM-CLI-Commands)
	* CLI Commands Links
		* [outdated](https://github.com/NinjiaHub/NPM-CLI-Commands/blob/master/documents/npm-outdated.md)
		* [npm-profile](https://github.com/NinjiaHub/NPM-CLI-Commands/blob/master/documents/npm-profile.md)

## 注释

### 关于CLI Commands

其实npm CLI Commands属于npm工具，是npm的一部分，但是在刚开写npm这部分时计划根据自己的使用经验以及完整看官方文档写一个总结+笔记的东西，后来再完整看npm CLI Commands这部分时，感觉作为一个完整的翻译应该比较好，所以npm CLI Commands这部分作为一个单独的仓库拆出去了，但是依然写在了npm的推荐阅读部分，也算是全面掌握npm时需要熟练掌握的部分。

## 声明

该目录下的文章，有的是笔记，有的是自己写的总结；部分内容来自网络或者书籍，如果涉及版权问题，请联系作者。

侵删。

多数文章中的内容是在学习的过程中记录的，所以难免有错误、不准确和不恰当的地方，有问题的地方请斧正，请提`Merge Request`给作者。

作者邮箱：web.taox@mail.com。

## Author Info

* [GitHub](https://github.com/Tao-Quixote)
* Email: web.taox@gmail.com
