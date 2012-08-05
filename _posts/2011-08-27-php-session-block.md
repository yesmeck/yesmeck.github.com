--- 
comments: true
layout: post
title: "Session引起的PHP脚本阻塞"
category: 求生技能
tags: PHP
date: 2011-08-27 21:25:12 +08:00
---
上回说要改opencart其实是给opencart加一个抓取淘宝上的产品的功能，但是弄完后发现一个问题，就是当脚本在抓取的时候，因为这个过程比较慢，这个时候其他所有脚本的执行都被阻塞了，直到抓取完其他脚本才能依次执行。研究了半天没有结果，在知乎上问了下可能是session的问题，需要调用session_write_close()来解决，那么这个session_write_close()是干嘛用的呢，手册上这样写的：

> 结束当前session，保存session数据。
>
> session数据通常会在脚本执行结束后被保存而并不需要调用session_write_close(),但是为保护session在任何时候都只能被一个脚本执行写操作，session的数据会被锁住。当同时使用框架网页和session时你会发现，框架里的网页会因为这个个锁定而逐个载入。你可以通过在所有的session数据修改保存结束后马上结束session来加快载入时间。

这就很好的解释了为什么我的抓取脚本会阻塞其他页面的原因。所以，如果你有一个需要执行时间比较长并用到session的ajax请求的话，就需要在服务器端调用<a href="http://cn.php.net/manual/en/function.session-write-close.php" target="_blank">session_write_close()</a>，不然你的其他页面就都会被挂起直到请求结束！！！
