---
layout: post
title: "解决Arch启动时udev超时的问题"
date: 2012-01-28 15:17
comments: true
categories: 瞎折腾
---
前几天开始系统启动的时候总会在udev这一步停很长一段时间，直到超时。解决办法是把无线网卡模块加到``/etc/rc.conf``里。

{% highlight bash %}
# HARDWARE
# --------
MODULES=(rtl8192ce)
USEDMRAID="no"
USEBTRFS="no"
USELVM="no"
{% endhighlight %}

这里我电脑用的模块是``rtl8192ce``，如果你不确定自己的网卡用的什么模块可以用``kmod list | grep rtl``来查看。我的是这样：

{% highlight bash %}
meck@meck-laptop02 ~/Playground/blog.yesmeck.com
yesmeck* $ kmod list | grep rtl                                                                     [15:27:34]
rtl8192ce              73047  0
rtlwifi                91358  1 rtl8192ce
rtl8192c_common        56668  1 rtl8192ce
mac80211              229016  3 rtl8192c_common,rtlwifi,rtl8192ce
cfg80211              172260  2 mac80211,rtlwifi
usbcore               146209  9 ehci_hcd,usbhid,rtlwifi,btusb,usb_storage,ums_realtek,uvcvideo,uas
{% endhighlight %}
你可以到[wiki](https://wiki.archlinux.org/index.php/Wireless#Drivers_and_firmware)查看有哪些无线模块。

参考：https://alderaan.archlinux.org/viewtopic.php?pid=1045259
