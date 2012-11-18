---
layout: post
title: "博客折腾记"
description: ""
category: {}
tags: [jekyll, 折腾]
---
{% include JB/setup %}

### 还没折腾好
早就想写日志了，先是花钱盘下域名，四处寻找空间直到看到[这篇](http://www.douban.com/people/emptymalei/status/1018017583/)，一番google后，才知道世上还有github pages这等神奇的东西。

当然，因为我是bitbucket.org那边的（私有免费有木有）。研究了一下，发现它们只有username.bitbucket.org域名，而CNAME只能到转到bitbucket.org/username，关于[如何设置这个域名](https://confluence.atlassian.com/display/BITBUCKET/Publishing+a+Website+on+Bitbucket)底下的[评论](https://confluence.atlassian.com/download/attachments/221449776/subdirectory-discussion.pdf?version=1&modificationDate=1340380831909)里，明确的讲了CNAME到username.bitbucket.org这个东西暂时不会有。于是我就光荣地加入了github（才300M我会到处说吗？）。

github pages极其简单，我照bitbucket的方式，建了一个username.github.com仓库，然后点admin，Options>GitHub Pages>Automatic Page Generator一点，跟着提示，很轻松就好了（初始化要10分钟）。

用自有域名也很简单，在DNS管理里加CNAME到username.github.com，比如CNAME blog username.github.com，在username.github.com仓库里加一个CNAME文件，里面填CNAME的域名，比如blog.yourdomain.net。什么，A怎么办，我是新手我会到处乱说吗？

至此，全站搭建完毕。

### 待续

<s>还有什么jekyll bootstrap，octopress，disqus之类的以后再折腾吧。</s>

还有[这个](http://zespia.tw/Octopress-Theme-Slash/)很赞，先存着。

### debug

好像不支持`<>`标签
