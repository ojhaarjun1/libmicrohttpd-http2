---
title: Configuration
permalink: /configuration/
---

* TOC
{:toc}

## I want to use HTTP/2 in my MHD server!

In your MHD application, just include the ``microhttpd_http2.h``
header and add the ``MHD_USE_HTTP2`` flag when creating the MHD_Daemon.
And you're done! The use of HTTP2 is transparent to the application.

{% highlight c %}
#include <microhttpd_http2.h>

[..]

struct MHD_Daemon *d;

d = MHD_start_daemon (MHD_USE_HTTP2 |
                      MHD_USE_AUTO | MHD_USE_INTERNAL_POLLING_THREAD,
                      port,
                      NULL, NULL, &request_cb, NULL,
                      MHD_OPTION_END);
{% endhighlight %}

## HTTP/2 settings

When a new client connects to the server, they exchange their HTTP2 settings.

Settings parameters and their default values are detailed in
[RFC 7540](https://tools.ietf.org/html/rfc7540#section-6.5.2), but
you can easily modify them.

When creating the server, you need to pass the MHD_OPTION_HTTP2_SETTINGS option
and it must be followed by two arguments:
 - An integer of type `size_t`, which indicates the number of
   h2_settings_entry.
 - A pointer to a `h2_settings_entry` structure, an array of http2
   settings.
Note that the application must ensure that the buffer of the
second argument remains allocated and unmodified while the
deamon is running.

The complete parameter list for the h2_settings_entry structure is
[detailed here](https://nghttp2.org/documentation/enums.html?#c.nghttp2_settings_id).

Example:

{% highlight c %}
const int slen = 1;
h2_settings_entry h2_settings[] = {
    { .settings_id = NGHTTP2_SETTINGS_MAX_CONCURRENT_STREAMS,
      .value = 100 },
};

d = MHD_start_daemon (MHD_USE_HTTP2 |
                      MHD_USE_AUTO | MHD_USE_INTERNAL_POLLING_THREAD,
                      port,
                      NULL, NULL, &request_cb, NULL,
                      MHD_OPTION_HTTP2_SETTINGS, slen, h2_settings,
                      MHD_OPTION_END);
{% endhighlight %}
