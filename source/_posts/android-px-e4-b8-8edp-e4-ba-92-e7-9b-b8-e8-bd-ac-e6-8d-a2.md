title: "android px与dp互相转换"
date: 2014-04-01 16:09:41
tags:
id: 618
comment: false
categories:
  - 编程学习
---

<pre class="brush:java">public class DensityUtil {  

	    /** 
	     * 根据手机的分辨率从 dp 的单位 转成为 px(像素) 
	     */  
	    public int dip2px(Context context, float dpValue) {  
	        final float scale = context.getResources().getDisplayMetrics().density;  
	        return (int) (dpValue * scale + 0.5f);  
	    }  

	    /** 
	     * 根据手机的分辨率从 px(像素) 的单位 转成为 dp 
	     */  
	    public int px2dip(Context context, float pxValue) {  
	        final float scale = context.getResources().getDisplayMetrics().density;  
	        return (int) (pxValue / scale + 0.5f);  
	    }  
	}</pre>
Integer i = test.dip2px(this, 320);

textview.setText(i.toString());

TextView显示int类型数据。

&nbsp;