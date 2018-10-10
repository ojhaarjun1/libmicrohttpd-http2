---
title: Install
permalink: /install/
---

* TOC
{:toc}

## Download the source code

Download the latest version from the [releases page](https://github.com/maru/libmicrohttpd-http2/releases).

## Compile

Configure libmicrohttpd with the ``--enable-http2`` option, compile and install it:

    $ ./configure --enable-http2
    $ make && sudo make install

If libnghttp2 is not in a default library directory, explicitly state the path:

    $ ./configure --enable-http2 --with-nghttp2=/path/to/libnghttp2
