---
title: Discnt
layout: default
---

Discnt provides in memory distributed eventually consistent counters.

Setup
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

API
===

Discnt API is composed of a small set of commands, since the system solves a
single very specific problem. The main commands are:

    INCRBYFLOAT counter_name increment

Increments the counter by the specified increment. If the counter does not exist, it is set to 0 before performing the operation. 
If the command is successful the new incremented value is stored and returned to the caller as a string.
The precision of the output is fixed at 17 digits after the decimal point regardless of the actual internal precision of the computation.

This command is compatible with the Redis INCRBYFLOAT command.

    INCR counter_name

Same as `INCRBYFLOAT counter_name 1`. Provided for Redis compatibility.

This command is compatible with the Redis INCRBYFLOAT command.

    INCRBY counter_name increment

Same as `INCRBYFLOAT counter_name increment`. Provided for Redis compatibility.

This command is compatible with the Redis INCRBY command.

    DECR counter_name

Same as `INCRBYFLOAT counter_name -1`. Provided for Redis compatibility.

This command is compatible with the Redis 

    DECRBY counter_name decrement

This command is compatible with the Redis DECRBY command.

Same as `INCRBYFLOAT counter_name -decrement`. Provided for Redis compatibility.

    SET counter_name value

Set a counter to the specified value.

A SET also resets the local prediction to 0 and broadcasts it.

This command is compatible with the Redis SET command as long as a numeric value is used.

    GET counter_name [STATE]

Get the value of the counter. If the counter does not exist 0 is returned.
The precision of the output is fixed at 17 digits after the decimal point regardless of the actual internal value.
If the optional STATE is specified GET returns a multi reply containing the value and either the string CONSISTENT or
INCONSISTENT depending on the cluster state.

This command is compatible with the Redis GET command.

    PRECISION counter_name [value]

Set or get the precision for the local counter. See [Predictions](#predictions).

    SUBSCRIBE counter [counter ...]

Subscribes the client to the specified counters. Updates to the counters are published to the client with a minimum interval of once per second.

Once the client enters the subscribed state it is not supposed to issue any other commands, except for additional SUBSCRIBE, ISUBSCRIBE, and UNSUBSCRIBE commands.

This command is compatible with the Redis SUBSCRIBE command.

    ISUBSCRIBE interval counter [counter ...]

This command is the same as SUBSCRIBE but allows you to specify the minimum interval in seconds.

    UNSUBSCRIBE [channel [channel ...]]

Unsubscribes the client from updates to the given counters, or from all of them if none is given.

When no counters are specified, the client is unsubscribed from all the previously subscribed counters. In this case, a message for every unsubscribed counter will be sent to the client.

This command is compatible with the Redis UNSUBSCRIBE command.


Other commands
===

Note: not everything implemented yet.

    INFO

Generic server information / stats.

    HELLO

Returns hello format version, this node ID, all the nodes IDs, IP addresses,
ports, and priority (lower is better, means node more available).
Clients should use this as an handshake command when connecting with a
Discnt node.

    KEYS patten

Similar to redis KEYS.

    CLUSTER MEET ip port

CLUSTER MEET is used in order to connect different Discnt nodes into a working cluster.
See [Redis CLUSTER MEET](http://redis.io/commands/cluster-meet)

    CLUSTER NODES ip port

See [Redis CLUSTER NOTES](http://redis.io/commands/cluster-nodes)


Client libraries
===

Discnt uses the same protocol as Redis itself. To adapt Redis clients, or to use it directly, should be pretty easy. However note that Discnt default port is 5262 and not 6379.


Predictions
===

Discnt uses predictions to lower network uses between instances.

Predictions are done on the local shard of a counter. Once per second the prediction is checked against the real value of the local shard. If the prediction is still within the precision of the counter nothing needs to be done.
If it doesn't match a new prediction is made and the local value and prediction are send to all nodes. Predictions are done over a period of `history-size` seconds.
The default precision can be set using the `default-precision` config directive. Per counter precision can be get and set using the `PRECISION` command.


FAQ
===

Is Discnt part of Redis?
---

No, it is a standalone project, however a big part of the Redis networking source code, nodes message bus, libraries, and the client protocol, were reused in this new project.

However while it is a separated project, conceptually Discnt is related to Redis, since it tries to solve a Redis use case in a vertical, ad-hoc way.

Who created Discnt?
---

Discnt is a side project of Erik Dubbelboer.

What does Discnt means?
---

DIStributed CouNTers.


