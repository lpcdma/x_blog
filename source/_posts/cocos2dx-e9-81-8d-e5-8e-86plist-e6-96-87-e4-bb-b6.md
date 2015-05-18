title: "cocos2dx遍历plist文件"
date: 2014-06-27 18:48:01
tags:
id: 684
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">auto dict = Dictionary::createWithContentsOfFile("shapedefs.plist");
    //dict-&gt;acceptVisitor(vistor);

	__Dictionary* bodiesDict = (__Dictionary*)dict-&gt;objectForKey("bodies");
	Array* shitBodies = dynamic_cast&lt;Array*&gt;(dict-&gt;objectForKey("bodies"));

	for ( auto iter = shitBodies-&gt;begin(); iter != shitBodies-&gt;end(); ++iter) {

		Array* x = dynamic_cast&lt;Array*&gt;(*iter);
		for ( auto str = x-&gt;begin(); str != x-&gt;end(); ++str) {
			String* c = dynamic_cast&lt;String*&gt;(*str);
			log("%s", c-&gt;getCString());
		}

	log("-------------------------------");
	}

	bodiesDict-&gt;acceptVisitor(vistor);
	log("%s", vistor.getResult().c_str());
    log("-------------------------------");
	__Dictionary* drinkDict = (__Dictionary*)bodiesDict-&gt;objectForKey("drink");
	Array* test = (Array* )drinkDict-&gt;objectForKey("fixtures");
	__Dictionary *fixtureData = dynamic_cast&lt;__Dictionary*&gt;(*test-&gt;begin());

	int tag = static_cast&lt;__String *&gt;(fixtureData-&gt;objectForKey("density"))-&gt;intValue();

	//__Dictionary* tmp = test[0];
	Array* polygonsDict = dynamic_cast&lt;Array*&gt;(fixtureData-&gt;objectForKey("polygons"));
	//Array* polygonsDict = (Array* )drinkDict-&gt;objectForKey("polygons");
	//__Dictionary *polygonsData = dynamic_cast&lt;__Dictionary*&gt;(*polygonsDict-&gt;begin());
	for ( auto iter = polygonsDict-&gt;begin(); iter != polygonsDict-&gt;end(); ++iter) {

		Array* x = dynamic_cast&lt;Array*&gt;(*iter);
		for ( auto str = x-&gt;begin(); str != x-&gt;end(); ++str) {
			String* c = dynamic_cast&lt;String*&gt;(*str);
			log("%s", c-&gt;getCString());
		}

	log("-------------------------------");
	log("%d", tag);
    //log("%s", vistor.getResult().c_str());
    log("-------------------------------");
	}</pre>