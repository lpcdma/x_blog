title: "cocos2dx layer颜色渐变"
date: 2014-06-10 15:49:23
tags:
id: 669
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">    auto s = Director::getInstance()-&gt;getWinSize();
    auto layer1 = LayerColor::create( Color4B(255, 255, 0, 255), 100, 300); //Color4B(r,g,b,a),A为透明度。
    layer1-&gt;setPosition(Vec2(s.width/3, s.height/2));
    layer1-&gt;ignoreAnchorPointForPosition(false);//<span style="color: #525252;">对于场景或层等大型节点，它们的IgnoreAnchorPointForPosition属性为 true ，此时引擎会认为 AnchorPoint 永远为(0,0)；</span>
    addChild(layer1, 1);

    auto layer2 = LayerColor::create( Color4B(0, 0, 255, 255), 100, 300);
    layer2-&gt;setPosition(Vec2((s.width/3)*2, s.height/2));
    layer2-&gt;ignoreAnchorPointForPosition(false);
    addChild(layer2, 1);

    auto actionTint = TintBy::create(2, -255, -127, 0); //创建一个变化颜色的目标颜色。
    auto actionTintBack = actionTint-&gt;reverse();
    auto seq1 = Sequence::create( actionTint, actionTintBack, NULL);
    layer1-&gt;runAction(seq1);

    auto actionFade = FadeOut::create(2.0f);
    auto actionFadeBack = actionFade-&gt;reverse();
    auto seq2 = Sequence::create(actionFade, actionFadeBack, NULL);        
    layer2-&gt;runAction(seq2);</pre>