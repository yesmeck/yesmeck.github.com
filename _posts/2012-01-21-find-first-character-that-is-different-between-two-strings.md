---
layout: post
title: "找出两个字符串中第一个不同的字符位置"
date: 2012-01-21 05:39
comments: true
categories: 求生技能
---

这是一个在stackoverflow上的[问题](http://stackoverflow.com/questions/7475437/find-first-character-that-is-different-between-two-strings)。
给出两个长度相等的字符串，找出这两个字符串中第一个不同的字符位置。
一般的做法就会这样
{% highlight php %}
<?php
for ($offset = 0; $offset < $length; ++$offset) {
    if ($str1[$offset] !== $str2[$offset]) {
        return $offset;
    }
}
{% endhighlight %}
而问题下面给出的最佳答案是用异或操作符( ^ )，以前从来没用过这个操作符，也不知道能用到什么地方，今天算是学到。

因为一般情况下，当你对两个字符串进行异或操作的时候，相同的字符的异或结果是null("\0")，所以我们只要找出第一个非null("\0")字符就可以了。
{% highlight php %}
<?php
$position = strspn($string1 ^ $string2, "\0");
{% endhighlight %}
很明显这是一个更优雅高效的方法。
另外，回答的人还附加了一个多字节字符的解决办法。
{% highlight php %}
<?php
function getCharacterOffsetOfDifference($str1, $str2, $encoding = 'UTF-8') {
    return mb_strlen($str1, $encoding)
           - mb_strlen(
                 mb_strcut(
                     $str1,
                     strspn($str1 ^ $str2, "\0"),
                     mb_strlen($str1, '8bit'),
                     $encoding
                 ),
                 $encoding
             );
}
{% endhighlight %}
