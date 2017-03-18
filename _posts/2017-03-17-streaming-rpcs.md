---
layout: single
title:  "Streaming RPC's using gRPC"
date:   2017-03-17 16:00:00
categories: grpc python
---

### A quick demo of bi-directional streaming RPC's using gRPC, Go and Python

[gRPC] is a language-neutral, platform-neutral RPC framework that is quickly taking on JSON/HTTP as the recommended way to
communicate between microservices.

Its main selling points are:

* Communication is based on HTTP2 transport. This provides all the advantages of the HTTP2 (compression, multiplexing, streaming)
* Well-defined schemas via Protocol Buffers 3
* Automatic client code generation for 10 different languages.
* Bi-directional streaming

By default, gRPC uses Protocol Buffers as the Interface Definition Language (IDL) and as its underlying message interchange format.

In this project, we'll implement a simple PrimeFactorService that returns a stream of the prime factors of the numbers passed to
it in a request. I decided to make my gRPC server in Go and client in Python. The client program will read from stdin and will
immediately push it to the service.

### Generate gRPC code for server and client

We are going to use gRPC to generate libraries for Go and Python 3. To generate the Go code, you'll need to install [protoc].

{% highlight python %}
# Python client
$ pip3 install -U grpcio grpcio-tools
$ python3 -m grpc_tools.protoc -I protobuf/ --python_out=. --grpc_python_out=. protobuf/primefactor.proto
# Go
$ protoc -I protobuf/ --go_out=plugins=grpc:protobuf/ protobuf/primefactor.proto
{% endhighlight %}

The first command will generate primefactor_pb2.py and primefactor_pb2_grpc.py. The latter will generate primefactor.pb.go.

To start the server, simply run:

{% highlight bash %}
go run server.go
{% endhighlight %}

You can run the client using:

{% highlight bash %}
python3 client.py
{% endhighlight %}

![demo](https://github.com/ridha/grpc-streaming-demo/raw/master/demo.gif)

You can find the full source at [https://github.com/ridha/grpc-streaming-demo/]

### Additional resources:

* [gRPC overview]
* [gRPC motivation and design principles]
* [Protobufs]
* [Bidirectional gRPC streaming for Go]
* [gRPC Python tools]

[gRPC]: http://www.grpc.io/
[protoc]: https://github.com/google/protobuf/#protocol-compiler-installation
[gRPC overview]: http://www.grpc.io/docs/
[gRPC motivation and design principles]: http://www.grpc.io/blog/principles
[Protobufs]: https://developers.google.com/protocol-buffers/docs/proto3
[Bidirectional gRPC streaming for Go]: https://rakyll.org/grpc-streaming/
[gRPC Python tools]: https://pypi.python.org/pypi/grpcio-tools
[https://github.com/ridha/grpc-streaming-demo/]: https://github.com/ridha/grpc-streaming-demo/
