+++
date = "2013-10-17T00:00:00+08:00"
slug = "install-mj-in-ksp-022"
title = "KSP 0.22 中安装 Mechjeb"
lastmod = "2016-02-12T18:05:31+08:00"
publishDate = "2016-02-12T18:05:31+08:00"
+++

今天开Steam，收到了 Kerbal Space Program 0.22 版的更新。

> 2013年10月27日补充
>    
> Mechjeb已经发布2.1版，已经官方兼容 KSP 0.22 版了。

最大的改变就是原先的生涯模式现在可以尝试了。  
进去后发现多了一条很长的科技树，需要通过每次发射后，航天员或所携带的科学测量仪器带回来的数据，来提升科技点数，解锁新的航天部件。

但 Mechjeb 好像还没有为0.22更新，找了半天这货居然连官网都没怎么做，就一个Wiki，还没有下载地址。翻了半天终于在KSP的官方论坛找到了一个帖子，看到了下载地址，应该是MJ持续更新的。

1. 先下载 [Mechjeb 2.0.9 的最新开发版 (MechJeb2-2.0.9.0-82 ) ](http://jenkins.mumech.com/job/MechJeb2/lastSuccessfulBuild/artifact/jenkins-MechJeb2-82/MechJeb2-2.0.9.0-82.zip) ；
2. 将压缩包中的 `MechJeb2` 文件，解压缩至游戏目录中的 `GameData` 文件夹；
3. 采用任何一款编辑器（也可称作记事本），打开 `GameData\MechJeb2\Parts\MechJeb2_AR202\part.cfg` 文件，编辑其中`15行`的 `TechRequired` 属性的值为 `start` ，随后保存；
4. 进入游戏，开启生涯模式即可找到MJ的模块了（在控制组中）；
