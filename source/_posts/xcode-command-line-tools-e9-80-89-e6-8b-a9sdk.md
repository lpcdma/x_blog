title: "xcode command line tools选择sdk"
date: 2014-09-20 12:24:07
tags:
id: 734
comment: false
categories:
  - 学习笔记
---

<pre class="brush:shell">xcrun swift -v -sdk $(xcrun --show-sdk-path --sdk iphonesimulator)</pre>