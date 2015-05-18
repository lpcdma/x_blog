title: "cocos2dx事件(含C++11 Lambda函数说明)"
date: 2014-06-10 16:39:40
tags:
id: 671
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">Vec2 origin = Director::getInstance()-&gt;getVisibleOrigin(); //获得游戏界面开始的点。
    Size size = Director::getInstance()-&gt;getVisibleSize();

	log("origin,x=%d,orgin.y=%d",origin.x,origin.y);

    auto containerForSprite1 = Node::create();
    auto sprite1 = Sprite::create("Images/CyanSquare.png");
    sprite1-&gt;setPosition(origin+Vec2(size.width/2, size.height/2) + Vec2(-80, 80));
    containerForSprite1-&gt;addChild(sprite1);
    addChild(containerForSprite1, 10);

    auto sprite2 = Sprite::create("Images/MagentaSquare.png");
    sprite2-&gt;setPosition(origin+Vec2(size.width/2, size.height/2));
    addChild(sprite2, 20);

    auto sprite3 = Sprite::create("Images/YellowSquare.png");
    sprite3-&gt;setPosition(Vec2(0, 0)); //这是中心？
    sprite2-&gt;addChild(sprite3, 1);

    // Make sprite1 touchable
    auto listener1 = EventListenerTouchOneByOne::create();
    listener1-&gt;setSwallowTouches(true);

    listener1-&gt;onTouchBegan = [](Touch* touch, Event* event){
        auto target = static_cast&lt;Sprite*&gt;(event-&gt;getCurrentTarget());

        Vec2 locationInNode = target-&gt;convertToNodeSpace(touch-&gt;getLocation());
        Size s = target-&gt;getContentSize();
        Rect rect = Rect(0, 0, s.width, s.height);

        if (rect.containsPoint(locationInNode))
        {
            log("sprite began... x = %f, y = %f", locationInNode.x, locationInNode.y);
            target-&gt;setOpacity(180);
            return true;
        }
        return false;
    };

    listener1-&gt;onTouchMoved = [](Touch* touch, Event* event){
        auto target = static_cast&lt;Sprite*&gt;(event-&gt;getCurrentTarget());
        target-&gt;setPosition(target-&gt;getPosition() + touch-&gt;getDelta());
    };
	/*
•[] 不截取任何变量
•[&amp;} 截取外部作用域中所有变量，并作为引用在函数体中使用
•[=] 截取外部作用域中所有变量，并拷贝一份在函数体中使用
•[=, &amp;foo] 截取外部作用域中所有变量，并拷贝一份在函数体中使用，但是对foo变量使用引用

•[bar] 截取bar变量并且拷贝一份在函数体重使用，同时不截取其他变量
•[this] 截取当前类中的this指针。如果已经使用了&amp;或者=就默认添加此选项。
http://blog.csdn.net/srzhz/article/details/7934652
	*/

    listener1-&gt;onTouchEnded = [=](Touch* touch, Event* event){
        auto target = static_cast&lt;Sprite*&gt;(event-&gt;getCurrentTarget());
        log("sprite onTouchesEnded.. ");
        target-&gt;setOpacity(255);
        if (target == sprite2)
        {
            containerForSprite1-&gt;setLocalZOrder(100);
        }
        else if(target == sprite1)
        {
            containerForSprite1-&gt;setLocalZOrder(0);
        }
    };

    _eventDispatcher-&gt;addEventListenerWithSceneGraphPriority(listener1, sprite1);
    _eventDispatcher-&gt;addEventListenerWithSceneGraphPriority(listener1-&gt;clone(), sprite2);
    _eventDispatcher-&gt;addEventListenerWithSceneGraphPriority(listener1-&gt;clone(), sprite3);

    auto removeAllTouchItem = MenuItemFont::create("Remove All Touch Listeners", [this](Ref* sender){
        auto senderItem = static_cast&lt;MenuItemFont*&gt;(sender);
        senderItem-&gt;setString("Only Next item could be clicked");

        _eventDispatcher-&gt;removeEventListenersForType(EventListener::Type::TOUCH_ONE_BY_ONE);

        auto nextItem = MenuItemFont::create("Next", [=](Ref* sender){
			HelloWorld::menuCloseCallback(this);
        });

        nextItem-&gt;setFontSizeObj(16);
        nextItem-&gt;setPosition(VisibleRect::right() + Vec2(-100, -30));

        auto menu2 = Menu::create(nextItem, NULL);
        menu2-&gt;setPosition(Vec2(0, 0));
        menu2-&gt;setAnchorPoint(Vec2(0, 0));
        this-&gt;addChild(menu2);
    });

    removeAllTouchItem-&gt;setFontSizeObj(16);
    removeAllTouchItem-&gt;setPosition(VisibleRect::right() + Vec2(-100, 0));

    auto menu = Menu::create(removeAllTouchItem, nullptr);
    menu-&gt;setPosition(Vec2(0, 0));
    menu-&gt;setAnchorPoint(Vec2(0, 0));
    addChild(menu);</pre>