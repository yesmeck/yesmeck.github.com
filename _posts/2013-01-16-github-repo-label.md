---
layout: post
title: "GitHub 仓库页面左上角的标签"
description: ""
category:
tags: []
---
{% include JB/setup %}

GitHub 每个仓库页面，仓库名的左边都会有一个标签显示这个仓库是公开还是私有。

平时这个标签是这个样子的：

![GitHub Repo Label1](http://dl.dropbox.com/u/1658623/github-repo-label1.png)

而当你拖动浏览器，浏览器宽度变小的时候，它就会变成这个样子：

![GitHub Repo Label2](http://dl.dropbox.com/u/1658623/github-repo-label2.png)

一直很好奇这是怎么实现的，用 js 监听浏览器 resize 事件？感觉不大可能用这么土的方法。

前几天看到网上有人问怎么实现响应式设计，刚好这个词听过很多次了，但是一直不知道是个什么东西，今天研究了一下，原来 GitHub 的那个标签就是一个响应式设计的例子。

Responsive desgin，听名字很唬人，稍微研究一下其实实现起来很简单。主要靠的技术就是 Media Queries :

{% highlight html %}
<link rel="stylesheet" type="text/css" href="style.css" media="screen" />
<link rel="stylesheet" type="text/css" href="print.css" media="print" />
{% endhighlight %}

上面那个`media="print"`就是 Media Queries，浏览会根据这个属性为不同的设备加载不同的 css 文件。而在 CSS3 里，这个 Media Queries 得到了加强，现在可以这么写了：

{% highlight css %}
#container {
  background-color: black;
}

/* 等于小于 980px 的屏幕 */
@media screen and (max-width: 980px) {
  #container {
    background-color: white;
  }
}
{% endhighlight %}

直接在写 css 的时候就可以定义哪些样式作用于哪些 media，甚至还可以写条件。

所以 GitHub 那个标签的实现就可以想通了，顺便做了一个 [Demo](/demo/github-repo-label.html) 验证了一下。
