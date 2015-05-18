title: "cocostudio动画"
date: 2014-06-16 12:51:21
tags:
id: 673
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">ArmatureDataManager::getInstance()-&gt;addArmatureFileInfo("xxoo.ExportJson");
	Armature* armature = Armature::create("xxoo");
	//armature-&gt;getAnimation()-&gt;playWithIndex(2);
	armature-&gt;getAnimation()-&gt;play("fuck");
	armature-&gt;setPosition(Vec2(Director::getInstance()-&gt;getOpenGLView()-&gt;getVisibleRect().origin.x + Director::getInstance()-&gt;getOpenGLView()-&gt;getVisibleRect().size.width/2,Director::getInstance()-&gt;getOpenGLView()-&gt;getVisibleRect().origin.y + Director::getInstance()-&gt;getOpenGLView()-&gt;getVisibleRect().size.width/2));
	addChild(armature);</pre>