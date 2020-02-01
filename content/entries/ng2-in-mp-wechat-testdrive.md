+++
date = "2016-06-25T00:00:00+08:00"
slug = "ng2-in-mp-wechat-testdrive"
title = "微信公众号 Angular2 实测"
lastmod = "2016-06-25T18:33:02+08:00"
publishDate = "2016-06-25T17:47:35+08:00"
+++

最近干了一个微信公众号的活儿。  
业内现在好像挺流行React的，不过看了几天发现根本不适合我，毕竟单靠一个React似乎还搞不出SPA，不像NG是一站式解决。

开始建好Component，写好视图，配好路由器。Webpack一编译一切正常，TypeScript写起来就是舒服。
往服务器上一传，自己手机上（iOS 9）一看，一切运转正常。可没高兴几天，突然发现Android上居然连 app.component 都没法初始化。

第一反应先看看前辈们是否也遇到这个问题了吧。随后就看到了这个[提问](http://www.zhihu.com/question/41020291)。
一路搜索下去，看到了[这个Issue](https://github.com/angular/zone.js/issues/328) 。
瞬间感觉胜利就在眼前了，可惜改完往服务器上一传一点变化也没有，还是没法初始化。看来这样的黑箱操作是没法解决这问题了。

思前想后，看来方案有三种可选：

1. 各种 Hack，让 Angular 2 可以运行在腾讯的X5浏览器上
2. 判断 UserAgent ，为X5内核的浏览器单独做一个版本
3. 完全重做，考虑用 Angular1 或者 Vue.js 实现

想方设法，借到了一个可以连接PC端微信调试器的安卓机（联想A330E）。伴随着设备的到位，终于看到了 Javascript Console 抛出的异常。似乎是 ng2 内部的一个 Collection 对象的方法不支持。
这个问题搜了一下，似乎没什么好答案。悲剧啊，难道这注定了NG2无缘在微信浏览器里运行了么？

顺手翻了一下 NG2 官方的入门教程，发现他的页面中引入了一个 `core-js/client/slim.js` 的文件，一查原来是一个为了兼容低版本浏览器的库。也管不了这么多了，先引入进来再说吧。

直接修改入口文件：

~~~ csharp
import "core-js/client/shim";  // Add this import
import { AppComponent } from "./app.component";
import { bootstrap } from "@angular/platform-browser-dynamic";

bootstrap(AppComponent);
~~~

编译完往服务器上一更新，我勒个去，终于可以正常运行了。真是激动地差点出去放挂鞭炮。
由此看来，当前版本的微信 (iOS: 6.3.21 ，Android: 6.3.18) 可以完全正常的兼容Angular 2 (2.0.0-rc.2) 了。
