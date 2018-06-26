---
permalink: /push-server/
title: Writing a PUSH server
---

To start a server, you need to create it first. Use:

```
struct MHD_Daemon *d;

d = MHD_start_daemon (MHD_USE_AUTO | MHD_USE_INTERNAL_POLLING_THREAD | MHD_USE_ERROR_LOG,
                      port,
                      NULL, NULL, &echo_cb, PAGE,
                      MHD_OPTION_CONNECTION_TIMEOUT, (unsigned int) 120,
                      MHD_OPTION_END);
```

[back](./)
