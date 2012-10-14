---
layout: post
title: "Ruby 中隐藏的$变量和他们的意思"
description: ""
category:
tags: []
---
{% include JB/setup %}

Ruby 中充满了一系列的隐藏变量，我们可以从这么预定义的全局变量中获取一些有意思的信息。

## 全局进程变量

`$$` 表示当前运行的 ruby 进程。

{% highlight bash %}
>> $$
=> 17170
{% endhighlight %}

我们可以从当前进程杀死它自己

{% highlight bash %}
>> `kill -9 #{$$}`
[1]    17170 killed     irb
{% endhighlight %}

`$?` 表示最近一个子进程的状态

{% highlight bash %}
>> `echo hello`
=> "hello\n"
>> $?
=> #<Process::Status: pid 18048 exit 0>
>> $?.success?
=> true
{% endhighlight %}

## 异常和错误

`$1` 表示引起异常的信息。比如在这里 `raise "there's no peanut butter"`，它的值就是 `there's no peanut butter`。

{% highlight bash %}
>> begin raise "this town ain't big enough for the both of us" rescue puts $! end
this town ain't big enough for the both of us
=> nil
{% endhighlight %}

`$@` 可以给出完整的引起错误的栈调用信息，它是一个数组。

{% highlight bash %}
>> begin raise 'no soup in kitchen' rescue $@.each { |trace| puts trace } end
(irb):13:in `irb_binding'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/workspace.rb:80:in `eval'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/workspace.rb:80:in `evaluate'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/context.rb:254:in `evaluate'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:159:in `block (2 levels) in eval_input'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:273:in `signal_status'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:156:in `block in eval_input'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/ruby-lex.rb:243:in `block (2 levels) in each_top_level_statement'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/ruby-lex.rb:229:in `loop'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/ruby-lex.rb:229:in `block in each_top_level_statement'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/ruby-lex.rb:228:in `catch'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/ruby-lex.rb:228:in `each_top_level_statement'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:155:in `eval_input'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:70:in `block in start'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:69:in `catch'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:69:in `start'
/home/meck/.rvm/rubies/ruby-1.9.3-p194/bin/>>'
=> ["(>>'"]
{% endhighlight %}

## 字符串和分隔符

`$;` 表示 `String.split` 里的分隔符，默认是空格。

{% highlight bash %}
>> "One Spaceship, Two Tiny Tanks, Three Misplaced Socks".split
=> ["One Spaceship", " Two Tiny Tanks", " Three Misplaced Socks"]
>> $; = ","
=> ","
>> "One Spaceship, Two Tiny Tanks, Three Misplaced Socks".split
=> ["One Spaceship", " Two Tiny Tanks", " Three Misplaced Socks"]
{% endhighlight %}

`$,` 用在 `Array.join` 和 `Kernel.print` 里，默认是 nil。

{% highlight bash %}
>> ['one', 'two', 'three', 'green'].join
=> "onetwothreegreen"
>> $, = "-"
=> "-"
>> ['one', 'two', 'three', 'green'].join
=> "one-two-three-green"
{% endhighlight %}

`$/` 表述读取输入的行分隔符。它被用在 `Kernel.gets` 里。它通常表示新行，但可以被修改。这个很难展示，因为 `irb` 依赖 `\n` 作为读取分隔符，如果把 $/ 设置成 nil，`gets` 就会读取整个文件。

`$\` 正好相反，它是作为输出的行分隔符。

{% highlight bash %}
>> $\ = "mooooooo "
=> "mooooooo "
>> puts a
NameError-: -undefined local variable or method `a' for main:Object-
mooooooo        from (irb):25
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/bin/>>'-
mooooooo >> a
{% endhighlight %}

## 文件

假设有个叫`letter.text`的文件：

{% highlight %}
Dear Caroline,
I think we need some honey for tea.
I also think that I may have misplaced my red tie, have you seen it?

-Nick
{% endhighlight %}

`$.` 表示文件当前被读取的行号。

{% highlight bash %}
>> open('letter.txt').each { |line| puts "#{$.}: #{line}" }
1: Dear Caroline,
2: I think we need some honey for tea.
3: I also think that I may have misplaced my red tie, have you seen it?
4:
5: -Nick
=> #<File:letter.txt>
{% endhighlight %}

`$_` 表示最后读取的行。

{% highlight bash %}
>> open('letter.txt').each { |line| puts $_.nil? }
true
true
true
true
true
=> #<File:letter.txt>
{% endhighlight %}

## 匹配和正则表达式

`$~` 表示最近一次正则匹配到的信息，如果有的话它就返回 `MatchData` 的示例，否则就是 `nil`。

{% highlight bash %}
>> "the robots are coming, the robots are coming, the robots are coming" =~ /ro/
=> 4
>> $~
=> #<MatchData "ro">
>> $~.to_s
=> "ro"
>> "the robots are coming, the robots are coming, the robots are coming" =~ /cats/
=> nil
>> $~
{% endhighlight %}

`$&` 跟 `$~` 非常相似，它返回最近一次匹配到的字符串。

{% highlight bash %}
>> "the robots are coming, the robots are coming, the robots are coming" =~ /ro/
=> 4
>> $&
{% endhighlight %}

`$'` 表示匹配不分后面的字符串。

{% highlight bash %}
>> "There were once ten tin robots standing in a row." =~ /robot/
=> 24
>> $'
=> "s standing in a row."
=> "ro"
=> nil
{% endhighlight %}

## 其他

`$>` 表示ruby 默认的输出对象，用在 `Kernel.print` 里。

{% highlight bash %}
>> $> =  $> = $stderr
=> #<IO:<STDERR>>
>> puts 'no no no'
no no no
=> nil
>> $> = $stdin
/home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:168:in `write': not opened for writing (IOError)
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:168:in `print'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:168:in `block (2 levels) in eval_input'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:273:in `signal_status'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:156:in `block in eval_input'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/ruby-lex.rb:243:in `block (2 levels) in each_top_level_statement'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/ruby-lex.rb:229:in `loop'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/ruby-lex.rb:229:in `block in each_top_level_statement'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/ruby-lex.rb:228:in `catch'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb/ruby-lex.rb:228:in `each_top_level_statement'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:155:in `eval_input'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:70:in `block in start'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:69:in `catch'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/lib/ruby/1.9.1/irb.rb:69:in `start'
        from /home/meck/.rvm/rubies/ruby-1.9.3-p194/bin/irb:12:in `<main>'
{% endhighlight %}

`$*` 可能是最常用全局变量，它表示包含传给 ruby 文件的所有变量的数组，假设有个叫 `argument_echoer.rb` 文件：

{% highlight ruby %}
$*.each { |arg| puts arg }
{% endhighlight %}

运行它：

{% highlight bash %}
$ ruby argument_echoer.rb Who What When Where and Why
Who
What
When
Where
and
Why
{% endhighlight %}

原文：http://blog.dcxn.com/2012/09/16/cryptic-dollar-variables-and-their-meanings/
