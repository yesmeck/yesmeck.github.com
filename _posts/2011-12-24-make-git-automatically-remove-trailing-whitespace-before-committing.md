---
layout: post
title: "Make git automatically remove trailing whitespace before committing"
date: 2011-12-24 01:13
comments: true
categories: 求生技能
---

Last post I wrote a simple script to make git check trailing whitespace
before committing.Today, I improve the script to make git automatically
remove whitespace for us.

Here it is.

{% highlight bash %}
#!/bin/sh
#

# A git hook script to find and fix trailing whitespace
# in your commits. Bypass it with the --no-verify option
# to git-commit
#

# Find files with trailing whitespace
for file in `git diff --check --cached | grep '^[^+-]' | grep -o '^.*[0-9]\+:'` ; do
    file_name=`echo ${file} | grep -o '^[^:]\+'`
    line_number=`echo ${file} | grep -oP '(?<=:)[0-9]+(?=:)'`
    (sed -i "${line_number}s/\s*$//" ${file_name} > /dev/null 2>&1 \
        || sed -i '' -E "${line_number}s/\s*$//" ${file_name})
    git add ${file_name}
done

# Now we can commit
exit
{% endhighlight %}
