---
layout: single
title:  "Compiling Windows binaries on Linux"
date:   2008-11-25 03:39:00
categories: linux
---
The idea is how to setup your Linux box to compile your code for the Windows platform using [MinGW](https://en.wikipedia.org/wiki/MinGW).
Depending on your distribution, the installation of the needed packages can differ.

### Installing Mingw

#### Ubuntu 7.10

Install the mingw package from the Universe Repository, using synaptics or with the following command:

{% highlight bash %}
sudo apt-get install mingw32
{% endhighlight %}

### Example Usage

Once installed, save the following file as hello.c (stolen from [Installing and Using the MinGW Cross-Compiler on Mac OS X]):

{% highlight c %}
#include <windows.h>
int main(int argc, char *argv[])
{
    MessageBox(NULL, "Hello, world!", "Hello, world!", MB_OK);
    return 0;
}
{% endhighlight %}

To build the example, execute the following command:

{% highlight bash %}
i586-mingw32msvc-gcc hello.c -o hello.exe
{% endhighlight %}

and run it, for example, with wine: wine hello.exe

[Installing and Using the MinGW Cross-Compiler on Mac OS X]: http://landonf.bikemonkey.org/code/win32/MinGW.20041207231336.1583.sulu.html
