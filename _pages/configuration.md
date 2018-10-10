---
title: Configuration
---

* TOC
{:toc}

## I want to use HTTP/2 with my MHD server!

If you already have a MHD application, you just need to add the HTTP2 flag
when creating the MHD_Daemon. And you're done!

```
struct MHD_Daemon *d;

d = MHD_start_daemon (MHD_USE_AUTO | MHD_USE_INTERNAL_POLLING_THREAD | MHD_USE_ERROR_LOG | MHD_USE_HTTP2,
                      port,
                      NULL, NULL, &request_cb, NULL,
                      MHD_OPTION_END);
```

The use of HTTP2 is transparent to the application.


## HTTP/2 settings

HTTP/2 settings of the daemon, which are sent when a new client connection
occurs. This option should be followed by two arguments:
 - An integer of type `size_t`, which indicates the number of
   nghttp2_settings_entry.
 - A pointer to a `nghttp2_settings_entry` structure, an array of http2
   settings.
Note that the application must ensure that the buffer of the
second argument remains allocated and unmodified while the
deamon is running.
Settings parameters and their default values are defined in
https://tools.ietf.org/html/rfc7540#section-6.5.2

The list of settings to pass in the nghttp2_settings_entry structure is:
https://nghttp2.org/documentation/enums.html?#c.nghttp2_settings_id

```
nghttp2_settings_entry settings[3];
int slen = 0;
settings[slen].settings_id = NGHTTP2_SETTINGS_MAX_CONCURRENT_STREAMS;
settings[slen].value = 100;
++slen;

d = MHD_start_daemon (MHD_USE_AUTO | MHD_USE_INTERNAL_POLLING_THREAD | MHD_USE_ERROR_LOG | MHD_USE_HTTP2,
                      port,
                      NULL, NULL, &request_cb, NULL,
                      MHD_OPTION_H2_SETTINGS, slen, settings,
                      MHD_OPTION_END);
```

--encoder-header-table-size=<SIZE>
            Specify encoder header table size.  The decoder (client)
            specifies  the maximum  dynamic table  size it  accepts.
            Then the negotiated dynamic table size is the minimum of
            this option value and the value which client specified.
