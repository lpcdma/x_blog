title: "ListView与ScrollView冲突"
date: 2014-04-15 23:53:22
tags:
id: 652
comment: false
categories:
  - 学习笔记
---

<pre class="brush:java">public boolean dispatchTouchEvent(MotionEvent ev) {
		// TODO Auto-generated method stub
		if(ev.getAction() == MotionEvent.ACTION_MOVE){
			listView.dispatchTouchEvent(ev);
		}
		return super.dispatchTouchEvent(ev);
	}</pre>