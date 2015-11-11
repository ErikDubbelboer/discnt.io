---
title: API
layout: default
apimenu:
  - INCRBYFLOAT
  - INCR
  - INCRBY
  - DECR
  - DECRBY
  - SET
  - GET
  - MGET
  - PRECISION
  - SUBSCRIBE
  - ISUBSCRIBE
  - UNSUBSCRIBE
  - INFO
  - HELLO
  - KEYS
  - CLUSTER
---

{{ page.title }}
===

Discnt API is composed of a small set of commands, since the system solves a
single very specific problem. The main commands are:


<hr id="incrbyfloat">
<h4>INCRBYFLOAT</h4>

    INCRBYFLOAT counter increment

Increments the counter by the specified increment. If the counter does not exist, it is set to 0 before performing the operation. 
If the command is successful the new incremented value is stored and returned to the caller as a string.
The precision of the output is fixed at 17 digits after the decimal point regardless of the actual internal precision of the computation.

This command is compatible with the Redis INCRBYFLOAT command.


<hr id="incr">
<h4>INCR</h4>

    INCR counter

Same as `INCRBYFLOAT counter 1`. Provided for Redis compatibility.

This command is compatible with the Redis INCRBYFLOAT command.


<hr id="incrby">
<h4>INCRBY</h4>

    INCRBY counter increment

Same as `INCRBYFLOAT counter increment`. Provided for Redis compatibility.

This command is compatible with the Redis INCRBY command.

<hr id="decr">
<h4>DECR</h4>

    DECR counter

Same as `INCRBYFLOAT counter -1`. Provided for Redis compatibility.

This command is compatible with the Redis 


<hr id="decrby">
<h4>DECRBY</h4>

    DECRBY counter decrement

This command is compatible with the Redis DECRBY command.

Same as `INCRBYFLOAT counter -decrement`. Provided for Redis compatibility.


<hr id="set">
<h4>SET</h4>

    SET counter value

Set a counter to the specified value.

A SET also resets the local prediction to 0 and broadcasts it.

This command is compatible with the Redis SET command as long as a numeric value is used.


<hr id="get">
<h4>GET</h4>

    GET counter [STATE]

Get the value of the counter. If the counter does not exist 0 is returned.
The precision of the output is fixed at 17 digits after the decimal point regardless of the actual internal value.
If the optional STATE is specified GET returns a multi reply containing the value and either the string CONSISTENT or
INCONSISTENT depending on the cluster state.

This command is compatible with the Redis GET command.


<hr id="mget">
<h4>MGET</h4>

    MGET counter [counter ...]

Get the value of one or more counters. If a counter does not exist 0 is returned as its value.


<hr id="precision">
<h4>PRECISION</h4>

    PRECISION counter [value]

Set or get the precision for the local counter. See [Predictions](/#predictions).


<hr id="subscribe">
<h4>SUBSCRIBE</h4>

    SUBSCRIBE counter [counter ...]

Subscribes the client to the specified counters. Updates to the counters are published to the client with a minimum interval of once per second.

Once the client enters the subscribed state it is not supposed to issue any other commands, except for additional SUBSCRIBE, ISUBSCRIBE, and UNSUBSCRIBE commands.

This command is compatible with the Redis SUBSCRIBE command.


<hr id="isubscribe">
<h4>ISUBSCRIBE</h4>

    ISUBSCRIBE interval counter [counter ...]

This command is the same as SUBSCRIBE but allows you to specify the minimum interval in seconds.


<hr id="unsubscribe">
<h4>UNSUBSCRIBE</h4>

    UNSUBSCRIBE [channel [channel ...]]

Unsubscribes the client from updates to the given counters, or from all of them if none is given.

When no counters are specified, the client is unsubscribed from all the previously subscribed counters. In this case, a message for every unsubscribed counter will be sent to the client.

This command is compatible with the Redis UNSUBSCRIBE command.


<hr id="info">
<h4>INFO</h4>

    INFO

Generic server information / stats.


<hr id="hello">
<h4>HELLO</h4>

    HELLO

Returns hello format version, this node ID, all the nodes IDs, IP addresses,
ports, and priority (lower is better, means node more available).
Clients should use this as an handshake command when connecting with a
Discnt node.


<hr id="keys">
<h4>KEYS</h4>

    KEYS patten

Similar to redis KEYS.


<hr id="cluster">
<h4>CLUSTER</h4>

    CLUSTER MEET ip port

CLUSTER MEET is used in order to connect different Discnt nodes into a working cluster.
See [Redis CLUSTER MEET](http://redis.io/commands/cluster-meet)

    CLUSTER NODES ip port

See [Redis CLUSTER NOTES](http://redis.io/commands/cluster-nodes)

