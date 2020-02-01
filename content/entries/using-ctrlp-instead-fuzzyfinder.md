+++
date = "2013-05-23T00:00:00+08:00"
slug = "using-ctrlp-instead-fuzzyfinder"
title = "用 ctrlp.vim 代替 FuzzyFinder"
lastmod = "2016-02-12T18:04:27+08:00"
publishDate = "2016-02-12T18:04:27+08:00"
+++

话说VIM也用了一段时间，可是进展还不是大，一直被我当作一个`文本编辑器`在是用，惭愧唉。

本来在搞Yii项目，后来觉得PHP是误入歧途所以现在转投了我大Rails的旗下。Ruby都这么爽了不把VIM折腾爽了怎么对得起他呢。

本来用的`FuzzyFinder`来快速定位文件（这个功能完全是用了`Sublime Text 2`之后才发现又多么的高效），可是

1. 速度有时候会比较慢
2. 如果其他地方更改了文件名或文件夹则需要刷新缓存，这时又会卡一下
3. 再者如果打开了项目中子目录下的某个文件，则还需要手动跳会项目根目录

这样让我折腾了好久，才算是勉强能正常使用。无意间在Rails的论坛发现有人介绍`ctrlp`（观看地址 <http://happycasts.net/episodes/64> ）遂立刻动手，真的是很爽。

因为我的vimfiles已经通过Git实现控制，所以直接添加他的库为`submodule`既可。

    git submodule add https://github.com/kien/ctrlp.vim.git bundle/ctrlp.vim

其余参照 <http://kien.github.io/ctrlp.vim/#installation> 所提示，在 `.vimrc` 中添加 

    set runtimepath^=~/.vim/bundle/ctrlp.vim

即可正常使用。默认快捷键`Ctrl+P`。

一个字，爽。项目自动通过`.git`, `.hg`, `.svn` 等来判断根目录，随时都可以自由的在项目中查找文件；配置忽略设置也简单了很多。

> 我的vimfiles: <https://github.com/francistm/vim-files>
