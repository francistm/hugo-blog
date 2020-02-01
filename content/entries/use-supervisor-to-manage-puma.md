+++
date = "2015-07-30T00:00:00+08:00"
slug = "use-supervisor-to-manage-puma"
title = "用 Supervisor 管理 Puma 进程"
lastmod = "2016-02-12T19:34:44+08:00"
publishDate = "2016-02-12T18:08:12+08:00"
+++

不得不说，以前一直是直接用 capistrano 来跑 puma ，这样的做法实在是太乱搞了。
鉴于每次更新程序，重启 puma 的时候，你都不知道他是不是挂掉了，今天下了个狠心，用了个进程守护程序来管着 puma 。


因为小时候家里穷没钱上学，所以 bash 这种重要语言不会，只能找找简单点的了。 Ruby 圈里似乎没找到合适的东西，就直接上 Python 的吧。
看起来 supervisor 挺符合需求的，搞过来算了。

supervisor 的配置直接写成这样

~~~ini
[program:web-app]
user      = <username>
directory = /home/<username>/sites/xxx.com/current
command   = bundle exec puma -C /home/<username>/sites/xxx.com/current/config/puma.rb
~~~

本来想的挺简单，结果一跑哎我靠不运行。忽然想到又存在一个 rvm 这破玩意儿。因为是安装到当前的用户，所以以 root 身份运行的 supervisor 找不到也是可以理解的。
直接写一个绝对路径引用 rvm 中的 Ruby 好像也不行，因为还存在 bundler 查找 gem 的问题。

查了一下 bundler 和 rvm 的文档，看来似乎用 alias 就搞定了。

如果你没有用 `default` 的版本，那就先创建一个 `alias` 。

~~~bash
~# rvm alias my-app-ruby-rev ruby-2.2.1
~~~

这时候就可以通过调用 `/home/<username>/.rvm/wrapper/my-app-ruby-rev/bundle` 文件来通过 rvm 自动选择需要的版本了。

再修改一下 supervisor 的配置：

~~~ini
[program:web-app]
user      = web-app
directory = /absolute/path/to/app/current
command   = /home/<username>/.rvm/wrapper/my-app-ruby-rev/bundle exec puma -C ./relative/path/to/config
~~~

艾玛，终于跑起来了，整个人都觉得生活充满了希望。

话说这么一搞， capistrano 里的 deploy.rb 又可以简化一大片了， stop, start 的部分都可以删掉，只留下 restart 的部分，直接给 puma 的进程发送 SIGUSR2 就OK了。
实测了一下效果不错，异常稳定啊。

美洲狮目前来说终于被驯服了。
