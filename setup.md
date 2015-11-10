---
title: Setup
layout: default
---

{{ page.title }}
===

To play with Discnt please do the following:

1. Compile Discnt, if you can compile Redis, you can compile Discnt, it's the usual no external deps thing. Just type `make`. Binaries (`discnt-cli` and `discnt-server`) will end up in the `src` directory.
2. Run a few Discnt nodes in different ports. Create different `discnt.conf` files following the example `discnt.conf` in the source distribution.
3. After you have them running, you need to join the cluster. Just select a random node among the nodes you are running, and send the command `CLUSTER MEET <ip> <port>` for every other node in the cluster.

To run a node, just call `./discnt-server`.

For example if you are running three Discnt servers in port 7711, 7712, 7713 in order to join the cluster you should use the `discnt-cli` command line tool and run the following commands:

    ./discnt-cli -p 7711 cluster meet 127.0.0.1 7712
    ./discnt-cli -p 7711 cluster meet 127.0.0.1 7713

Your cluster should now be ready. You can try to add a job and fetch it back
in order to test if everything is working:

    ./discnt-cli -p 7711
    127.0.0.1:7711> INCRBY test 0.1
    0.1
    127.0.0.1:7711> GET test
    0.1

Remember that you can increment counters on different nodes as Discnt
is multi master.

