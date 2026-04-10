+++
date = "2013-03-18T00:00:00+08:00"
slug = "change-to-another-application-again"
title = "再度换Blog"
lastmod = "2016-02-15T14:37:22+08:00"
publishDate = "2016-02-12T18:03:55+08:00"
+++

唉，最近又犯了折腾的毛病，`PHP + MySQL` 似乎已经不能满足我了，PHP写了这么久实在没什么好感（关于语言设计上的问题我也没资格妄加评论），但对于`Python`什么的又无大爱；索性直接一步换到`Ruby on Rails`算了。

一直听说`Yii`和`Ruby on Rails`很相似，才鼓足勇气装好了`rvm`，开始准备尝试。一路上磕磕绊绊，途中几经放弃，但最终还是折腾出了这么一个破玩意儿。

不得不赞叹一下`Rails`这个神器，后悔早些年没有接触到他。和`Yii`相比较：

- 没有那一套内置的挂件，比如`CMenu`, `CGridView` 等，需要自己动手。但灵活性有显著提高
- 系统集成度不是很高，比如用户登录部分就需要自己手动实现
- 部署后，支持`CSS`以及`JavaScript`的压缩、合并
- 配置文件略显复杂，手册到目前为止还没有看懂

至于`Ruby`和`PHP`的比较，这里就不提了，爽的真的不只是一点点啊。

目前依旧部署在[OpenShift](http://openshift.redhat.com/)上，使用[Orca.IO](http://orca.io/)加速。

所用的gems

- sorcery 登录验证
- kaminari 翻页
- kramdown Markdown解析

用`http://rubygems.org`安装Gem速度实在让人崩溃，干脆本地的`rvm`换成淘宝的源，然后`bundle install --local`直接妥活，唉……
