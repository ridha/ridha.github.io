---
layout: single
title:  "Tab completion in Python interpreter"
date:   2010-02-18 17:02:00
categories: python
---

You can achieve auto completion inside Python interpreter by adding the following
lines to your .pythonrc file (or the file you have set Python to read on startup
using PYTHONSTARTUP environment variable):

{% highlight python %}
>>>import rlcompleter, readline
>>>readline.parse_and_bind(‘tab: complete’)
{% endhighlight %}

This will make Python complete partially typed function, method and variable names
when you press the tab key. You can also set the prompts sys.ps1 and sys.ps2 in this file.
