---
layout: post
title: "Jekyll折腾记"
description: ""
category: 
tags: [jekyll, 折腾, 未解之谜]
---
{% include JB/setup %}

自开始使用github pages以后，很久没有更新博客了。这个周末天气很不错，是矫正拖延症好日子，于是乎打开电脑，把这个博客折腾成真正的博客。

### 安装ruby

由于主力系统是ubuntu 12.04，也就是说机器上是有ruby的，可是按照jekyll的安装[教程](https://github.com/mojombo/jekyll/wiki/install)，`gem install jekyll`，却总是得到：

    "/usr/local/lib/ruby/1.9.1/yaml.rb:56:in'<top(required)>':
    It seems your ruby installation is missing psych (for YAML output). 
    To eliminate this warning, please install libyaml and reinstall your ruby."

于是`sudo apt-get install libyaml-dev`，依然存在这个问题。google之，发现了[这篇](http://zhan.renren.com/nonocd#feed_3602888498002804845)。按着步骤一路走下去，装了rvm，然后`rvm install ruby-1.9.1-dev`，居然要我给kernel打patch，无奈的放弃，`rvm install ruby-1.9.3-p0`，终于成功了，改了gnome-terminal的设置，`rvm use ruby-1.9.3-p0`，再`gem install jekyll`，不再提示libyaml的错误，欣喜之余，报了依赖问题错误。

无奈的从git上clone了ruby的[源码](https://github.com/ruby/ruby)，很正常的装上了，一装jekyll，还是有依赖问题。我认为是安装的姿势不对，参考[这个答案](http://stackoverflow.com/a/9158048)，结果姿势还是不对。

### 安装jekyll

想了想，可能是gem源的问题，`gem source -a http://ruby.taobao.org`，居然无法连接 （现在已经可以了），想到不久前看到了[这篇](http://ruby-china.org/topics/6520)，果断`gem source -a http://mirrors.tuna.tsinghua.edu.cn/rubygems/`，再装jekyll，终于成功了。

至此，问题解决，而这问题到底是什么造成的？是由于ubuntu残缺不全的ruby，还是斯巴达前后复杂的网络环境？我想，这大概是一个属于我的未解之谜。
