---
comments: true
layout: post
title: "阻止JavaScript报错"
category: 求生技能
tags: JavaScript
date: 2011-03-09 09:05:37 +08:00
---
总有一些js写得你内牛满面。

{% highlight javascript linenos %}
function BlockError()
{
    return true;
}
window.onerror = BlockError;
{% endhighlight %}
