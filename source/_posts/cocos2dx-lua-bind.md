title: "cocos2dx lua bind"
date: 2014-08-04 17:41:03
tags:
id: 702
comment: false
categories:
  - 学习笔记
---

按照[README.mdown](https://github.com/chukong/cocos-docs/blob/master/manual/framework/native/v2/lua/lua-binding-for-custom-class/zh.md "如何使用bindings-generator自动绑定自定义类")所提供的方法配置好环境。

修改tools\tolua目录下适合自己使用的文件，重命名保存备用。

复制一份genbindings.py，修改自己指定路径，注释掉多余的ini配置文件

注意：千万不要执行genbindings.py，因为执行后，新建的项目将无法编译通过。

新建cpp类
<pre class="brush:cpp">#ifndef __LUABUINDSUM_H__
#define __LUABUINDSUM_H__

#include "cocos2d.h"

class luabindsum
{
public:
	luabindsum();
	~luabindsum();
	int mysum(int x, int y);

private:

};

#endif
</pre>
<pre class="brush:cpp">#include "luabindsum.h"
#include "cocos2d.h"

int luabindsum::mysum(int x, int y)
{
	return x + y;
}

luabindsum::luabindsum()
{

}

luabindsum::~luabindsum()
{

}

</pre>
静态函数调用方法：
<pre class="brush:cpp">luabindsum.mysum(luabindsum,6, 16)</pre>
非静态函数调用方法：
<pre class="brush:cpp">local shit = luabindsum:new()
shit:mysum(3,4)

/*冒号代表 self */</pre>