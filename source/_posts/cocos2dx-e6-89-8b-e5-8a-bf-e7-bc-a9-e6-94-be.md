title: "cocos2dx 手势缩放"
date: 2014-06-24 14:46:51
tags:
id: 680
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">	void onTouchesBegan(const std::vector&lt;Touch*&gt;&amp;touches, cocos2d::Event* event);
	void onTouchesMoved(const std::vector&lt;Touch*&gt;&amp;touches, cocos2d::Event* event);
	void onTouchesEnded(const std::vector&lt;Touch*&gt;&amp;touches, cocos2d::Event* event);
	void onTouchCancelled(const std::vector&lt;Touch*&gt;&amp;touches, cocos2d::Event* event);
	float distance;
	float mdistance;
	float mscale;
	Sprite* sprite;
//添加事件函数和缩放所需变量。</pre>
在v3 的ontouchesbegin中捕获不到多点触控事件。

所以初始化缩放变量的初始值就存在一定的问题。暂时无解。
<pre class="brush:cpp">void HelloWorld::onTouchesMoved(const std::vector&lt;Touch*&gt;&amp;touches, cocos2d::Event* event){

	if( touches.capacity() &gt;= 2){
			auto itembegin = *touches.begin();
			auto itemended = *(touches.end()-1);
			auto itembeginLocation = itembegin-&gt;getLocation();
			auto itemendedLocation = itemended-&gt;getLocation();
			if(mscale == 1.0f){
				distance = sqrt((itembeginLocation.x-itemendedLocation.x)*(itembeginLocation.x-itemendedLocation.x) + (itembeginLocation.y-itemendedLocation.y)*(itembeginLocation.y-itemendedLocation.y));
				mscale = mscale+0.0001f;
			}else{
				mdistance = sqrt((itembeginLocation.x-itemendedLocation.x)*(itembeginLocation.x-itemendedLocation.x) + (itembeginLocation.y-itemendedLocation.y)*(itembeginLocation.y-itemendedLocation.y));
				mscale = mdistance / distance * mscale;
				distance = mdistance;
			}
			log("%d",distance);
			sprite-&gt;setScale(mscale);
		}
}</pre>
&nbsp;

&nbsp;

&nbsp;