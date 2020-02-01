+++
date = "2013-12-15T00:00:00+08:00"
slug = "define-secret-for-travis"
title = "Travis-CI 中定义敏感变量"
lastmod = "2016-02-14T16:17:05+08:00"
publishDate = "2016-02-12T18:05:56+08:00"
+++

最近无聊，折腾了一下Rails中上传文件到[七牛](http://www.qiniu.com)的功能。

因为`config/initializer/qiniu-rs.rb`文件（这里强烈吐槽一下，为什么其他文件都是以`_`分割单词，唯独七牛以`-`呢，好别扭看起来）中包含敏感的两个密钥值，所以就没有加入到版本库中。

本地测试时自己新建一个文件，讲七牛后台的两个值写进去即可，到了Travis-CI上就不行了，只可以完整的保留版本库中的文件，无法自行添加。直接将这两个值写到版本库里也是非常不合适的，毕竟账号可是自己的，网站也是自己的，不能瞎折腾。

Google上搜了一顿后，发现有一个`travis`的 [gem](https://rubygems.org/gems/travis)，可以提供一个`encrypt`的方法。

~~~bash
gem install travis
~~~

安装完成后运行

~~~bash
travis encrypt -a env.global AK=ACCESS_KEY SK=SECRET_KEY`
~~~

会自动将加密后的环境变量加入到`.travis.yml`中

在`before_script`中即可直接使用这两个变量了，例如

~~~bash
echo "$AK:$AF" > ./config
~~~

话说经过各种折腾，Travis-CI还是相当方便的！ 

> 七牛的测试账号可以在 [这里](http://developer.qiniu.com/docs/v6/kb/testing.html#upload) 里找到
