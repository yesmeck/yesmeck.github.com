--- 
comments: true
layout: post
title: "再记一个雅虎通的在线状态API"
category: 求生技能
tags: [YahooMessager, PHP]
date: 2011-04-13 09:08:15 +08:00
---
http://opi.yahoo.com/online?u=USERNAME&amp;m=g&amp;t=0

以上是直接返回在线状态的图标，t后面的参数可以是0-24，分别返回不同的图标样子

http://opi.yahoo.com/online?u=USERNAME&amp;m=a

以上是返回这样的文本信息：USERNAME is NOT ONLINE


http://opi.yahoo.com/online?u=USERNAME&amp;m=a&amp;t=1

以上返回状态代码，00表示不在线，01表示在线
