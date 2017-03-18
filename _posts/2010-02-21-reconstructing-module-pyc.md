---
layout: single
title:  "Reconstructing a module from a Python pyc file"
date:   2010-02-21 20:21:00
categories: python
---

You can reconstruct configuration files from a .pyc version using 'marshal' and 'dis' modules.

{% highlight python %}
>>import marshal
>>from dis import dis
>>f = open("/tmp/config.pyc", "rb")
>>f.seek(8)
>>obj = marshal.load(f)
>>dis(obj)
  1           0 LOAD_CONST               0 (-1)
              3 LOAD_CONST               1 (None)
              6 IMPORT_NAME              0 (sys)
              9 STORE_NAME               0 (sys)

  2          12 LOAD_CONST               2 ('oracle')
             15 STORE_NAME               1 (db_platform)

  3          18 LOAD_CONST               3 ('akm@xasamail.com')
             21 LOAD_CONST               4 ('test@test.com')
             24 BUILD_LIST               2
             27 STORE_NAME               2 (mail_to)
             30 LOAD_CONST               1 (None)
             33 RETURN_VALUE
>>
{% endhighlight %}

And from this, one can see what this configuration module 'config.py' used to say:

{% highlight python %}
import sys
db_platform = 'oracle'
mail_to = ['akm@xasamail.com', 'test@test.com']
{% endhighlight %}
