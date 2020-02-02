+++
date = "2014-02-18T00:00:00+08:00"
slug = "how-to-join-cagetest"
title = "追寻 Cagetest 的招聘信息"
lastmod = "2016-02-14T07:25:08+08:00"
publishDate = "2016-02-12T18:06:31+08:00"
+++

无意中看到了一个网站，[CAGETEST](http://www.cagetest.com/joinus.html) ，高级渗透测试。听名字是挺高端，大气，上档次的。页面最下角找到了一个Join Us的链接，很有兴趣的看看成为黑客级的程序员需要哪些素质，就点开来看看。

打开后什么都没有，查看源码就发现一段疑似base64加密后的字符串。  
![](http://7xqvtj.com1.z0.glb.clouddn.com/uploads/files/51/cagetest-encoded-string.jpg?imageMogr/thumbnail/750x%3E)

用Base64解密后，果然是一片乱码，没有看到任何有用的字符串信息……  
这可怎么办，瞬间无从下手了。想想我当年高大上的Gitcafe不也是如此的招聘信息么。如今看来果然是花瓶一只，和Cagetest的这些黑客们比起来都是小儿科。

把解密的结果输出到文件，然后用VIM打开，然后换到二进制。突然发现第0~1的字节为`1f8b 0808`。好熟悉的文件头啊！  
Google一查，好像跟GZip有关系。直接用GZip解压了，果然没有任何错误。  
![](http://7xqvtj.com1.z0.glb.clouddn.com/uploads/files/61/cagetest-base64-decoded-output-header.png?imageMogr/thumbnail/750x%3E)

解压缩以后，又出来一个非文本的文件，唉，真是苦逼。难度可以不要这么高么。
继续分析文件头看看有没有什么门道。  
![](http://7xqvtj.com1.z0.glb.clouddn.com/uploads/files/71/cagetest-gzip-decompressed-output-header.png?imageMogr/thumbnail/750x%3E)

看了半天也没什么进展。不过就看到开头是NES三个字母，难道真的是任天堂的ROM？
查了一下<http://fms.komkon.org/EMUL8/NES.html#LABM>，好像还真的完全符合啊。仔细看了一遍这玩意儿，好像完全不懂，唉。

按照文档的大致思路，可能招聘的信息就在PPU段中。尝试了各种的逆向工程，都毫无进展。看来我真的不是这块料。
既然事已至此，就搞一个模拟器看看这ROM到底是什么内容，也验证一下自己的判断到底是不是NES的ROM吧。

拖进去一看，还真的运行了，就是没有声音，也没有办法操作。  

就这样卡了我两天。各种偏移都没有找到任何有用的信息。  
今天中午实在太饿，但一看钱包里只剩下一毛钱（没错，真的是一毛钱），还是算了吧。  
忽然回想了一下看到的那个画面，`NES -at- CAGETEST.COM` 这不就是一个Email地址么？  

应该这就是官方所提到的招聘信息吧？  
更或许还有更深层次的信息我没有挖掘出来，我的水平也就到这个地步了。
