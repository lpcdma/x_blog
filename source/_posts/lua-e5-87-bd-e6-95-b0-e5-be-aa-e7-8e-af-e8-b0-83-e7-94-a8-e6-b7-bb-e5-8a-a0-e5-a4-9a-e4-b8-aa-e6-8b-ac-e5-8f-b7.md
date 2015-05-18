title: "lua函数循环调用(添加多个括号)"
date: 2014-09-19 14:31:05
tags:
id: 727
comment: false
categories:
  - 编程学习
---

<pre class="brush:cpp">function g( str )
        local o_count = 0
        local function o_func( str )
                if ( str ) then
                        print('the o_func call '..o_count)
                else
                        o_count = o_count + 1
                        return o_func
                end
        end
        return o_func( str )
end

print(g()()()()()()()('x'))

--</pre>