+++
date = "2016-10-09T00:00:00+08:00"
slug = "upgrade-to-rails-5"
title = "升级至 Rails 5.0"
lastmod = "2016-10-09T03:24:16+08:00"
publishDate = "2016-10-09T03:23:51+08:00"
+++

眼看着今年也没搞出来什么东西，实在惭愧。就升级了下Rails5，体验下新功能吧。Blog功能过于简陋，所以最值得学习的ActionCable完全没有应用的地方。
 
1. 先修改 `Gemfile` ，把Rails5的版本换上来；
2. 执行 `bundle outdate` 看还有哪些包已经过期；
3. 修改完后直接 `bundle update`；

试着运行一下， 把一些 deprecated 的方法和配置更新了以后，可以试着 `rails s` 跑一下了。

看起来一切顺利，可RSpec怎么也跑不过。一看我勒个去，控制器测试语法都变了？以前的 `post :create, user: attributes_for(:user)` 已经改为 `post :create, params: { user: attributes_for(:user) }` 了？好吧，看起来没有什么自动化的方法，一个个的根据报错改吧。

好了看起来这次真的没什么问题了。实际一访问发现又出现各种诡异样式问题，原来Turbolinks的接口也变了。以前的 `$(document).on("page:load", callback)` 现在要换成 `$(document).on("turbolinks:load", callback)` 了。

以上这些都改完以后，再整体的看一遍，似乎没什么大问题了，终于可以推上去部署了。
