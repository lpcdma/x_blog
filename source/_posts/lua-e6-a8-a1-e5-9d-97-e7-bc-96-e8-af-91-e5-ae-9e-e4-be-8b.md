title: "lua模块编译实例"
date: 2014-07-23 12:43:40
tags:
id: 689
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">/*
g++  -Wextra -O2 -c -o fucklib.o fuck.cpp 
g++ -shared -o fucklib.so fucklib.o
编译可执行文件g++ -o lua lua.c -llua -lm -ldl
*/
#include &lt;stdio.h&gt;
#include &lt;string.h&gt;
#include &lt;lua.hpp&gt;
#include &lt;lauxlib.h&gt;
#include &lt;lualib.h&gt;

extern "C" int add(lua_State* L) 
{
    double op1 = luaL_checknumber(L,1);
    double op2 = luaL_checknumber(L,2);
    lua_pushnumber(L,op1 + op2);
    return 1;
}

extern "C" int sub(lua_State* L)
{
    double op1 = luaL_checknumber(L,1);
    double op2 = luaL_checknumber(L,2);
    lua_pushnumber(L,op1 - op2);
    return 1;
}

static luaL_Reg mylibs[] = { 
    {"add", add},
    {"sub", sub},
    {NULL, NULL} 
}; 

extern "C" int luaopen_fucklib(lua_State* L) 
{
    const char* libName = "mytestlib";
    luaL_register(L,libName,mylibs);
    return 1;
}</pre>