---
layout: post
title: "改变jekyll日期格式"
description: ""
category: how-to
tags: [jekyll, how-to]
---
{% include JB/setup %}

jekyll 默认的时间格式是英文的，形如`16 Aguest, 2014`，作为一个没有过四级的人看起来很不爽。

google了一番，这个格式是由函数决定的，默认的格式是`{% raw %}{{ post.date | date_to_string }}{% endraw %}`。可以给一个新的格式`{% raw %}{{ post.date | date: "Y-m-d" }}{% endraw %}`，就变成了`2014-8-16`，蛮简单的。

改掉`_includes/themes/bootstrap-3/post.html`，`_includes/JB/posts_collate`，就剩下`Archive`里的英文月份了，以后再说吧。

------

PS: clojure写多了，list老忘加逗号。jekyll里的tags就需要用逗号隔开，想想这样也对，要不这样，添一个带空格的tag多麻烦（好吧，带空格的tag也蛮奇怪的）。
