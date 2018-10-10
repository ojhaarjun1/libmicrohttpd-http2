---
title: Install
permalink: /install/
---

* TOC
{:toc}

## Download the source code

Download the latest version from the [releases page](https://github.com/maru/libmicrohttpd-http2/releases).

## Requirements

libmicrohttpd-http2 needs [nghttp2](https://github.com/nghttp2/nghttp2/releases)
(minimum v1.31.0).

## Compile

Configure libmicrohttpd with the ``--enable-http2`` option, compile and install it:

    ./configure --enable-http2
    make && sudo make install

If libnghttp2 is not in a default library directory, explicitly state the path:

    ./configure --enable-http2 --with-nghttp2=/path/to/libnghttp2
