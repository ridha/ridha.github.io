---
layout: single
title:  "Using Tornado for Asynchronous Downloads"
date:   2013-07-05 20:41:00
categories: python
---

[Tornado] is a powerful, scalable web server written in Python and it includes an HTTP client called AsyncHTTPClient,
which performs HTTP requests asynchronously.

Here is an example of how to get partial updates as the download progresses: pass in a streaming_callback to the HTTPRequest().

The streaming_callback will be called for each chunk of data from the server, and HTTPResponse.body and HTTPResponse.buffer will
be empty in the final response. The on_response will be called when the file has been fully fetched.

{% highlight python %}
class AsyncHttpDownload(object):

    def __init__(self, url, ioloop):
        self.ioloop = ioloop
        req = HTTPRequest(url=url, streaming_callback=self.streaming_callback)
        client = AsyncHTTPClient()
        client.fetch(req, callback=self.on_response)

    def streaming_callback(self, data):
        sys.stdout.write(data)

    def on_response(self, response):
        sys.stdout.flush()
        if response.error:
            print 'Error:', response.error
        self.ioloop.stop()


if __name__ == '__main__':
    url = r'http://httpbin.org/stream/25'
    ioloop = IOLoop.instance()
    AsyncHttpDownload(url, ioloop)
    ioloop.start()
{% endhighlight %}

[Tornado]: http://www.tornadoweb.org/
