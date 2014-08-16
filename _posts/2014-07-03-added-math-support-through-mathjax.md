---
layout: post
title: "使用MathJax为jekyll添加Latex支持"
description: ""
category: 
tags: [jekyll, mathjax]
---
{% include JB/setup %}

## 缘起

Jekyll Bootstrap是一个优秀的jekyll模板，但是标准的Jekyll Bootstrap模板并没有支持latex，这严重降低了一个科技博客的逼格——没有公式 can see？今天就来开个坑搞搞它。

## 要加个这货

[MathJax](http://www.mathjax.org/)是一款blablabla...（此处略去1000字）。介绍完了MathJax，谈谈个人的感受吧。第一次见到MathJax，是了解了一些$$\LaTeX$$，并震惊于$$\LaTeX$$环境配置之变态后，偶然在豆瓣的数学组发现了[一个帖子](http://www.douban.com/group/topic/20188812/)（阿尔法城链接已挂，RIP），借助优秀的firefox扩展[greasemonkey](https://addons.mozilla.org/zh-cn/firefox/addon/greasemonkey/)，可以将形如`[;\LaTeX;]`的代码渲染成：

$$\LaTeX$$

这太炫酷了，当时我就震惊了。 

## 如何添加

由于我使用kramdown作为markdown编译器（在`_config.yml`中加入`markdown: kramdown`），jekyll会自动使用kramdown对`.md`文件进行编译。对于位于段落中，形如`$$ \LaTeX $$`的代码块，会被编译为`<script type="math/tex">\LaTeX</script>`，显示为块级元素$$\LaTeX$$。

如果位于单独的一行，则会被编译为`<script type="math/tex; mode=display">\LaTeX</script>`，显示为行级元素：

$$\LaTeX$$

但是仅仅经过kramdown编译，不在html中加入相应库，浏览器还是不能正常识别显示的。这个库，就是MathJax啦。找到`_includes/themes/THEME/default.html`，在`<head>`中插入：

    <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>

大功告成。

----------

$$E=mc^2$$

$$\because \therefore科学道理，国家机密，不告诉你$$
