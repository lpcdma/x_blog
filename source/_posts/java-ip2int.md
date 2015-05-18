title: "java ip2int"
date: 2014-04-11 13:43:16
tags:
id: 637
comment: false
categories:
  - 编程学习
---

<pre class="brush:java">import java.net.InetAddress;
import java.nio.ByteBuffer;

// ...

try {
    // Convert from integer to an IPv4 address
    InetAddress foo = InetAddress.getByName("2130706433");
    String address = foo.getHostAddress();
    System.out.println(address);

    // Convert from an IPv4 address to an integer
    InetAddress bar = InetAddress.getByName("127.0.0.1");
    int value = ByteBuffer.wrap(bar.getAddress()).getInt();
    System.out.println(value);

} catch (Exception e) {
    e.printStackTrace();
}</pre>