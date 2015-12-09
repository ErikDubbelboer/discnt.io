---
title: Security
layout: default
---

{{ page.title }}
===

Discnt is designed to be accessed by trusted clients inside trusted environments. This means that usually it is not a good idea to expose the Discnt instance directly to the internet or, in general, to an environment where untrusted clients can directly access the Discnt TCP port or UNIX socket.

By default Discnt binds to all interfaces. It is possible to bind Discnt to a single interface by adding a line like the following to the discnt.conf file:

```
bind 127.0.0.1
```

Failing to protect the Discnt port from the outside can have a big security impact because of the nature of Discnt. For instance, a single `DEBUG SEGFAULT` command can be used by an external attacker to crash the instance.

For more information see the [Redis page on security](http://redis.io/topics/security)

