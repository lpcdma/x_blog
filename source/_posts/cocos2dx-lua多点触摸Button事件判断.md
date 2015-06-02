title: "cocos2dx lua多点触摸Button事件判断"
date: 2015-06-02 22:55:17
tags:
---
##代码如下
```lua
	if ccui.TouchEventType.ended == etype and 
    \#cc.Director:getInstance():getOpenGLView():getAllTouches() == 0 then
	end
```