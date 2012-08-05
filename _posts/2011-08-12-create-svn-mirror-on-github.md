---
comments: true
layout: post
title: "在GitHub上建立一个SVN仓库的镜像"
category: 求生技能
tags: [Git, GitHub, SVN]
date: 2011-08-12 23:21:23 +08:00
---
今天想改一个<a href="http://www.opencart.com" target="_blank">开源程序</a>，但是发现他用的是google code的<a href="http://opencart.googlecode.com/svn/" target="_blank">SVN</a>。想要做些修改，又想跟踪原作者的更新，但是又不想用SVN（用过Git就不会再想用SVN的了！），所以google了下怎么把SVN仓库同步到了GitHub上。下面记录一下步骤。

首先当然是在GitHub上建一个仓库，然后在本地建个Git目录并初始化：

{% highlight bash %}
$ mkdir ~/OpenCart
$ cd ~/OpenCart
$ git init
{% endhighlight %}

然后把SVN仓库添加为远程仓库

{% highlight bash %}
$ git svn init -T http://opencart.googlecode.com/svn/
{% endhighlight %}

fetch一下

{% highlight bash %}
$ git svn fetc
{% endhighlight %}

再用gc清理一下

{% highlight bash %}
$ git gc
{% endhighlight %}

把GitHub的仓库加进来，推上去

{% highlight bash %}
$ git remote add origin git@github.com:yesmeck/OpenCart.git
$ git push origin master
{% endhighlight %}

如果SVN仓库有更新的话就这样

{% highlight bash %}
$ git svn rebase
$ git push origin master
{% endhighlight %}

