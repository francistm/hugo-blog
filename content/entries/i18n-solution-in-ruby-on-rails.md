+++
date = "2014-03-16T00:00:00+08:00"
slug = "i18n-solution-in-ruby-on-rails"
title = "Ruby on Rails 中的 i18n 解决"
lastmod = "2016-02-14T10:25:59+08:00"
publishDate = "2016-02-12T18:07:05+08:00"
+++

不得不说，RoR还是相当完美的，真的所谓能想到的功能他都提供了。  
最近在搞老陈的站，考虑到外贸的原因所以要搞i18n功能。

需要解决：

1. 模型中字段的i18n
2. 根据用户的 `Accept-Language` 请求自动判断
3. 访问 en.domain.com 时，自动切换到英文站
4. 后台编辑时，添加不同语言而不添加重复产品

其实这些问题还是没什么难度，只是我刚入行Rails水平还是太差。自己瞎折腾了一段时间，还是弄出了方案。

1. 用 `globalize` [这个gem](https://github.com/globalize/globalize)
2. 用 `http_accept_language` [这个gem](https://github.com/iain/http_accept_language)
3. 基于第二点，在Nginx中，设置转发后端服务器，强制加上 `Accept-Language en,en-US` 请求头部
4. 基于第一、二、三点，通过访问 `en.domain.com/admin` 和 `zh.domain.com/admin` 来自动实现locale切换，添加不同语言的内容

基于上面几点，整个项目的核心问题得以顺利解决。  
因为老板的原因，代码就不公布出来了。
