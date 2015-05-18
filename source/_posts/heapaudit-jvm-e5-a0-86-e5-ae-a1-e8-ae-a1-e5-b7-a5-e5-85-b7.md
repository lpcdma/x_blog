title: "HeapAudit JVM堆审计工具"
date: 2014-03-03 11:31:47
tags:
id: 539
comment: false
categories:
  - 工具收集
---

HeapAudit is a java agent which audits heap allocations for JVM processes.

HeapAudit runs in three modes:

*   STATIC: This requires a simple integration hook to be implemented by the java process of interest. The callback hook defines how the allocations are recorded and the callback code is only executed when the java agent is loaded.
*   DYNAMIC: This injects HeapQuantile recorders to all matching methods and dumps heap allocations to stdout when removed. Be aware, a lot of recorders, including nested ones, may be injected if the supplied matching pattern is not restrictive enough.
*   HYBRID: This launches like the static use case but dynamically determines where to inject recorders.
Best way to understand what HeapAudit can do for you is to check out some of the sample [tutorials](https://github.com/foursquare/heapaudit/blob/master/src/test/java/com/foursquare/heapaudit/tutorials/) and play around with the examples!

[https://github.com/foursquare/heapaudit](https://github.com/foursquare/heapaudit)

&nbsp;