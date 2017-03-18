---
layout: single
title:  "Getting realtime output using Python Subprocess"
date:   2015-02-22 21:10:00
categories: python
---

Normally when you want to get the output of a unix process in Python you have to
wait until the process finishes. This is annoying if you are running a process that
takes a while to finish.

Here's a way to get the output in real-time using Python subprocess.

{% highlight python %}
import subprocess

class ReadUnBuffered(object):

    def __init__(self, cmd):
        assert cmd, 'Missing command line'
        self._cmd = cmd
        self._proc = None
        self._ok = True

    def __enter__(self):
        self._proc = subprocess.Popen(self._cmd,
                                      stdout=subprocess.PIPE,
                                      stderr=subprocess.STDOUT)
        return self

    def readline(self):
        line = self._proc.stdout.readline().rstrip()
        self._proc.poll()
        rc = self._proc.returncode
        self._ok = (rc is None or rc == 0)
        return line

    def __iter__(self):
        with self as p_in:
            while p_in:
                line = self.readline()
                if not line:
                    break
                yield line

    def __exit__(self, exc_type, exc_value, traceback):
        if self._proc.returncode is None:
            self._proc.terminate()
        self._proc.wait()


if __name__ == '__main__':
    cmd = ['ls', '-lt', '/tmp/']
    for line in ReadUnBuffered(cmd):
        print(line)
{% endhighlight %}
