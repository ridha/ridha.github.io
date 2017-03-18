---
layout: single
title:  "Quick file sharing using Python"
date:   2008-07-22 09:00:00
categories: python shell
---
First drop an alias -
{% highlight bash %}
alias qshare='python -c "import SimpleHTTPServer;SimpleHTTPServer.test()"' {% endhighlight %}
into your  bash profile.
Then whenever you  want to share the contents of a folder with somebody, changing
the directory to the appropriate folder and running  "qshare", you’ve got yourself a
webserver running on port 8000 with directory listing enabled.
