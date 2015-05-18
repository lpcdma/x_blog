title: "Cache interaction with memory system"
date: 2014-02-17 17:37:17
tags:
id: 503
comment: false
categories:
  - 学习笔记
---

This section describes how to enable or disable the cache RAMs, and to enable or disable error checking. After you enable or disable the instruction cache, you must issue an `ISB` instruction to flush the pipeline. This ensures that all subsequent instruction fetches see the effect of enabling or disabling the instruction cache.

After reset, you must invalidate each cache before enabling it.

When disabling the data cache, you must clean the entire cache to ensure that any dirty data is flushed to L2 memory.

Before enabling the data cache, you must invalidate the entire data cache if L2 memory might have changed since the cache was disabled.

Before enabling the instruction cache, you must invalidate the entire instruction cache if L2 memory might have changed since the cache was disabled.

See [_Enabling or disabling AXI slave accesses_](http://infocenter.arm.com/help/topic/com.arm.doc.ddi0363e/Babhieaj.html "9.5. Enabling or disabling AXI slave accesses") and [_Accessing RAMs using the AXI slave interface_](http://infocenter.arm.com/help/topic/com.arm.doc.ddi0363e/Babgcegj.html "9.6. Accessing RAMs using the AXI slave interface") for information about how to access the cache RAMs using the AXI slave interface.
<div lang="en" xml:lang="en">
<div>

#### <a id="id6191480"></a>Disabling or enabling all of the caches

</div>
The following code is an example of enabling caches:
<pre>MRC p15, 0, R1, c1, c0, 0  ; Read System Control Register configuration data</pre>
<pre>ORR R1, R1, #0x1 &lt;&lt;12      ; instruction cache enable</pre>
<pre>ORR R1, R1, #0x1 &lt;&lt;2       ; data cache enable</pre>
<pre>DSB</pre>
<pre>MCR p15, 0, r0, c15, c5, 0 ; Invalidate entire data cache</pre>
<pre>MCR p15, 0, r0, c7, c5, 0  ; Invalidate entire instruction cache</pre>
<pre>MCR p15, 0, R1, c1, c0, 0  ; enabled cache RAMs</pre>
<pre>ISB</pre>
The following code is an example of disabling the caches:
<pre>MRC p15, 0, R1, c1, c0, 0  ; Read System Control Register configuration data</pre>
<pre>BIC R1, R1, #0x1 &lt;&lt;12      ; instruction cache disable</pre>
<pre>BIC R1, R1, #0x1 &lt;&lt;2       ; data cache disable</pre>
<pre>DSB</pre>
<pre>MCR p15, 0, R1, c1, c0, 0  ; disabled cache RAMs</pre>
<pre>ISB</pre>
<pre>; Clean entire data cache. This routine will depend on the data cache size. It can be omitted if it is known that the data cache has no dirty data</pre>
</div>
<div lang="en" xml:lang="en">
<div>

#### <a id="id6191619"></a>Disabling or enabling instruction cache

</div>
The following code is an example of enabling the instruction cache:
<pre>MRC p15, 0, R1, c1, c0, 0  ; Read System Control Register configuration data</pre>
<pre>ORR R1, R1, #0x1 &lt;&lt;12      ; instruction cache enable</pre>
<pre>MCR p15, 0, r0, c7, c5, 0  ; Invalidate entire instruction cache</pre>
<pre>MCR p15, 0, R1, c1, c0, 0  ; enabled instruction cache</pre>
<pre>ISB</pre>
The following code is an example of disabling the instruction cache:
<pre>MRC p15, 0, R1, c1, c0, 0   ; Read System Control Register configuration data</pre>
<pre>BIC R1, R1, #0x1 &lt;&lt;12       ; instruction cache enable</pre>
<pre>MCR p15, 0, R1, c1, c0, 0   ; disabled instruction cache</pre>
<pre>ISB</pre>
</div>
<div lang="en" xml:lang="en">
<div>

#### <a id="id6191711"></a>Disabling or enabling data cache

</div>
The following code is an example of enabling the data cache:
<pre>MRC p15, 0, R1, c1, c0, 0  ; Read System Control Register configuration data</pre>
<pre>ORR R1, R1, #0x1 &lt;&lt;2</pre>
<pre>DSB</pre>
<pre>MCR p15, 0, r0, c15, c5, 0 ; Invalidate entire data cache</pre>
<pre>MCR p15, 0, R1, c1, c0, 0  ; enabled data cache</pre>
The following code is an example of disabling the cache RAMs:
<pre>MRC p15, 0, R1, c1, c0, 0  ; Read System Control Register configuration data</pre>
<pre>BIC R1, R1, #0x1 &lt;&lt;2</pre>
<pre>DSB</pre>
<pre>MCR p15, 0, R1, c1, c0, 0  ; disabled data cache</pre>
<pre>; Clean entire data cache. This routine will depend on the data cache size. It can be omitted if it is known that the data cache has no dirty data.</pre>
</div>
<div lang="en" xml:lang="en">
<div>

#### <a id="CBBGFCAG"></a>Disabling or enabling error checking

</div>
Software must take care when changing the error checking bits in the Auxiliary Control Register. If the bits are changed when the caches contain data, the parity or ECC bits in the caches might not be correct for the new setting, resulting in unexpected errors and data loss. Therefore the bits in the Auxiliary Control Register must only be changed when both caches are turned off and the entire cache must be invalidated after the change.

The following code is the recommended sequence to perform the change:
<pre>MRC p15, 0, r0, c1, c0, 0  ; Read System Control Register</pre>
<pre>BIC r0, r0, #0x1 &lt;&lt; 2      ; Disable data cache bit</pre>
<pre>BIC r0, r0, #0x1 &lt;&lt; 12     ; Disable instruction cache bit</pre>
<pre>DSB</pre>
<pre>MCR p15, 0, r0, c1, c0, 0  ; Write System Control Register</pre>
<pre>ISB ; Ensures following instructions are not executed from cache</pre>
<pre>; Clean entire data cache. This routine will depend on the data cache size. It can be omitted if it is known that the data cache has no dirty data (e.g. if the cache has not been enabled yet).</pre>
<pre>MRC p15, 0, r1, c1, c0, 1  ; Read Auxiliary Control Register</pre>
<pre>; Change bits 5:3 as needed</pre>
<pre>MCR p15, 0, r1, c1, c0, 1  ; Write Auxiliary Control Register</pre>
<pre>MCR p15, 0, r0, c15, c5, 0 ; Invalidate entire data cache</pre>
<pre>MCR p15, 0, r0, c7, c5, 0  ; Invalidate entire instruction cache</pre>
<pre>MRC p15, 0, r0, c1, c0, 0  ; Read System Control Register</pre>
<pre>ORR r0, r0, #0x1 &lt;&lt; 2      ; Enable data cache bit</pre>
<pre>ORR r0, r0, #0x1 &lt;&lt; 12     ; Enable instruction cache bit</pre>
<pre>DSB</pre>
<pre>MCR p15, 0, r0, c1, c0, 0  ; Write System Control Register</pre>
<pre>ISB

[http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.ddi0363e/Chdhgibd.html](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.ddi0363e/Chdhgibd.html)</pre>
</div>