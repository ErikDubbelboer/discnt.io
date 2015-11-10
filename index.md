---
title: Index
layout: default
---

Discnt
===

Discnt provides in memory distributed eventually consistent counters.


Predictions
===

Discnt uses predictions to lower network uses between instances.

Predictions are done on the local shard of a counter. Once per second the prediction is checked against the real value of the local shard. If the prediction is still within the precision of the counter nothing needs to be done.
If it doesn't match a new prediction is made and the local value and prediction are send to all nodes. Predictions are done over a period of `history-size` seconds.
The default precision can be set using the `default-precision` config directive. Per counter precision can be get and set using the `PRECISION` command.

