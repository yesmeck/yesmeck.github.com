---
comments: true
layout: post
title: "使用Git部署网站"
category: 求生技能
tags: Git
date: 2011-05-21 09:10:44 +08:00
---
原文:http://toroid.org/ams/git-website-howto

**建立本地仓库**

{% highlight bash %}
$ mkdir website && cd website
$ git init
Initialized empty Git repository in /home/ams/website/.git/
$ echo 'Hello, world!' > index.html
$ git add index.html
$ git commit -q -m "The humble beginnings of my web site."
{% endhighlight %}

**远程仓库**

假设你的网站跑在一个你能通过ssh连接的服务器上。

{% highlight bash %}
$ mkdir website.git && cd website.git
$ git init --bare
Initialized empty Git repository in /home/ams/website.git/
{% endhighlight %}

然后用post-receive钩子来自动check out最后一个版本到你的网站目录里（网站目录需要手动创建）。

{% highlight bash %}
$ mkdir /var/www/www.example.org
$ cat > hooks/post-receive
#!/bin/sh
GIT_WORK_TREE=/var/www/www.example.org git checkout -f
$ chmod +x hooks/post-receive
{% endhighlight %}

注意：网站目录对你的ssh帐号需要有写权限
然后把远程仓库添加到本地

{% highlight bash %}
$ git remote add web ssh://server.example.org/home/ams/website.git
$ git push web +master:refs/heads/master
{% endhighlight %}

**完工**

需要更新网站的时候只要push过去就可以了

{% highlight bash %}
$ git push web
{% endhighlight %}
