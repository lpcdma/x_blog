title: "cocos2d-x动画"
date: 2014-06-01 00:51:15
tags:
id: 660
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">//使用plist和png 
  SpriteBatchNode *spritebatch = SpriteBatchNode::create("AnimBear.png");
    SpriteFrameCache *frameCache = SpriteFrameCache::sharedSpriteFrameCache();
    frameCache-&gt;addSpriteFramesWithFile("AnimBear.plist");

    auto frame_sp = Sprite::createWithSpriteFrameName("bear1.png");
    frame_sp-&gt;setPosition(Vec2(visibleSize.width/2 + origin.x, visibleSize.height/2 + origin.y));
    this-&gt;addChild(frame_sp, 0);

    Vector&lt;SpriteFrame*&gt; animFrames;
    for (int i = 0; i &lt; 8; i++) {
    	char szImageFileName[32] = {0};
    	sprintf(szImageFileName,"bear%d.png",i+1);
    	log("%s",szImageFileName);
		SpriteFrame* frame = frameCache-&gt;spriteFrameByName(szImageFileName);
		animFrames.pushBack(frame);
	}

    Animation *animation = Animation::createWithSpriteFrames(animFrames,0.1f);
    frame_sp-&gt;runAction(RepeatForever::create( Animate::create(animation) ));</pre>