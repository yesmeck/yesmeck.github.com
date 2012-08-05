---
layout: post
title: "Use git pre-commit hook to check unwanted trailing whitespace"
date: 2011-12-15 23:13
comments: true
categories: 求生技能
---

Trailing whitespaces are always boring. So when we use git, we can set a pre-commit hook
to check this for us.

Put following shell to your `.git/hook` folder.
{% highlight bash %}
#!/bin/sh

red="\033[1;31m"
color_end="\033[0m"

# Check unwanted trailing whitespace or space/tab indents;

if [[ `git diff --cached --check` ]]; then
    echo -e ${red}Commit failed${color_end}
    git diff --cached --check
    exit 1
fi
{% endhighlight %}

Don't forget give execute permission to it.
{% highlight bash %}
$ chmod +x pre-commit
{% endhighlight %}

