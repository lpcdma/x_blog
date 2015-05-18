title: "Linux C延时sleep、usleep、nanosleep、select"
date: 2013-11-03 19:10:30
tags:
id: 374
comment: false
categories:
  - 学习笔记
---

*   <span style="font-family: 'DejaVu Serif', serif;">sleep</span>的精度是秒
*   <span style="font-family: 'DejaVu Serif', serif;">usleep</span>的精度是微妙，不精确
*   <span style="font-family: 'DejaVu Serif', serif;">select</span>的精度是微妙，精确
*   <pre class="brush:cpp">struct timeval delay;
delay.tv_sec = 0;
delay.tv_usec = 20 * 1000; // 20 ms
select(0, NULL, NULL, NULL, &amp;delay);</pre>*   <span style="font-family: 'DejaVu Serif', serif;">nanosleep</span>的精度是纳秒，不精确
*   <span style="font-family: 'DejaVu Serif', serif;">unix</span>、<span style="font-family: 'DejaVu Serif', serif;">linux</span>系统尽量不要使用<span style="font-family: 'DejaVu Serif', serif;">usleep</span>和<span style="font-family: 'DejaVu Serif', serif;">sleep</span>而应该使用<span style="font-family: 'DejaVu Serif', serif;">nanosleep</span>，使用<span style="font-family: 'DejaVu Serif', serif;">nanosleep</span>应注意判断返回值和错误代码，否则容易造成<span style="font-family: 'DejaVu Serif', serif;">cpu</span>占用率<span style="font-family: 'DejaVu Serif', serif;">100%</span>。