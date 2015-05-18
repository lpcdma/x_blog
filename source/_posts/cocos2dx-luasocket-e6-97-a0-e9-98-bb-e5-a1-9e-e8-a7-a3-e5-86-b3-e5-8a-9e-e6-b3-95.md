title: "cocos2dx luasocket无阻塞解决办法"
date: 2014-08-05 20:59:40
tags:
id: 705
comment: false
categories:
  - 编程学习
---

lua代码：
<pre class="brush:py">local layer = cc.Layer:create()
    socket = require("socket")
    local msg,err
    local recvt, sendt = {}, {}
    local host = "localhost"
    local port = 4444
    local ip = "172.246.16.129"
    local udp = socket.udp()
    udp:settimeout(0)
    assert(udp:sendto("testing", ip, port))
    function update()
        cclog("this is update")
--socket.select(recvt, sendt, 1)
        msg,err = udp:receivefrom()
        print(msg, err)
        if msg ~= nil then
            --layer:unscheduleUpdate()
        end
    end</pre>
服务器代码
<pre class="brush:py">import socket
import time
address = ('0.0.0.0', 4444)
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.bind(address)
while True:
    data, (addr, port) = s.recvfrom(2048)
    time.sleep(20)
    s.sendto("AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA", (addr, port))
    if not data:
        print "client has exist"
        break
    print "received:", data, "from", addr

s.close()</pre>
使用定时器实现无诸塞。
<pre class="brush:py">layer:scheduleUpdateWithPriorityLua(update,0)</pre>
&nbsp;