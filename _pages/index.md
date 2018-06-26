---
layout: default
permalink: /
title: libmicrohttpd-http2
---

An HTTP/2 implementation for GNU libmicrohttpd.

## HTTP/2

[HTTP/2](https://tools.ietf.org/html/rfc7540) is the lastest version of the
HTTP protocol.
Its main feature is the use of a single connection between a client and a server.
Major changes have been introduced in HTTP/2, such as frames,
multiplexing, flow control, prioritization and
[header compression](https://tools.ietf.org/html/rfc7541),
making it very different (even incompatible) from its predecessor HTTP/1.

## MHD

[GNU libmicrohttpd](https://www.gnu.org/software/libmicrohttpd/) (MHD) is a small and
fast C library that implements HTTP server functionality. Some of its
implementation characteristics include: different threading models,
a reentrant API, fully HTTP/1.0 and HTTP/1.1 compliant.

MHD can be used in different platforms and in embedded systems without thread support,
thanks to its small binary size and high performance.

## The project: MHD + HTTP/2

The goal of this project is to add HTTP/2 support to MHD.

The HTTP/2 stack is already implemented by an existing library
[nghttp2](https://github.com/nghttp2/nghttp2),
used by well-known projects such as [curl](https://github.com/curl/curl).
I will use this library for the low level protocol handling parts and write
the functionality to handle the HTTP/2 connections.

MHD has a tightly coupled implementation of HTTP/1,
so adding HTTP/2 support is not straightforward.
I will create an interface between the TCP connection and the HTTP protocol,
in order to have an easy and transparent way to switch between protocols
(especially to support an HTTP/2 upgrade).

Follow the project at [github](https://github.com/maru/libmicrohttpd-http2)!
