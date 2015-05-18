title: "SSDP，简单服务发现技术"
date: 2013-12-11 11:25:22
tags:
id: 438
comment: false
categories:
  - 学习笔记
---

SSDP:Simple Sever Discovery Protocol,简单服务发现协议，此协议为网络客户提供一种无需任何配置、管理和维护网络设备服务的机制。此协议采用基于通知和发现路由的多播发现方式实现。协议客户端在保留的多播地址：239.255.255.250：1900（IPV4）发现服务，（IPv6 是：FF0x::C）同时每个设备服务也在此地址上上监听服务发现请求。如果服务监听到的发现请求与此服务相匹配，此服务会使用单播方式响应。

常见的协议请求消息有两种类型，第一种是服务通知，设备和服务使用此类通知消息声明自己存在；第二种是查询请求，协议客户端用此请求查询某种类型的设备和服务。请求消息中包含设备的特定信息或者某项服务的信息，例如设备类型、标识符和指向设备描述文档的URL地址。下图显示这两类通知消息和HTTP协议的关系：

&nbsp;

![](http://images.cnblogs.com/cnblogs_com/debin/188363/1.gif)

设备发现过程允许控制点使用一个设备类型或标识，或者是服务类型进行查询。这要求标准设备或服务类型，或者设备特定实例的发现和广告消息基于一个独一无二的标识，UPnP设备和服务类型的定义是UPnP论坛工作委员会的责任。从设备获得响应的内容基本上与多址传送的设备广播相同，只是采用单址传送方式。

SSDP 协议消息

1、设备查询消息

当一个控制点加入到网络中时，设备发现过程允许控制点寻找网络上感兴趣的设备。发现消息包括设备的一些特定信息或者某项服务的信息，例如它的类型、标识符、和指向XML设备描述文档的指针。从设备获得响应从本质上说，内容与多址传送的设备广播相同，只是采用单址传送方式。设备查询通过HTTP协议扩展M-SEARCH方法实现的。典型的设备查询请求消息格式：

&nbsp;
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<pre>M-SEARCH * HTTP/1.1
HOST: 239.255.255.250:1900
MAN: "ssdp:discover"
MX: seconds to delay response
ST: search target</pre>
</td>
</tr>
</tbody>
</table>
&nbsp;

各HTTP协议头的含义简介：

HOST：设置为协议保留多播地址和端口，必须是：239.255.255.250:1900（IPv4）或FF0x::C(IPv6)

MAN：设置协议查询的类型，必须是：ssdp:discover

MX：设置设备响应最长等待时间，设备响应在0和这个值之间随机选择响应延迟的值。这样可以为控制点响应平衡网络负载。

ST：设置服务查询的目标，它必须是下面的类型：

ssdp:all 搜索所有设备和服务
upnp:rootdevice 仅搜索网络中的根设备
uuid:device-UUID 查询UUID标识的设备
urn:schemas-upnp-org:device:device-Type:version 查询device-Type字段指定的设备类型，设备类型和版本由UPNP组织定义。
urn:schemas-upnp-org:service:service-Type:version 查询service-Type字段指定的服务类型，服务类型和版本由UPNP组织定义。

&nbsp;

&nbsp;

在设备接收到查询请求并且查询类型（ST字段值）与此设备匹配时，设备必须向多播地址239.255.255.250:1900回应响应消息。典型：

&nbsp;
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<pre>HTTP/1.1 200 OK
CACHE-CONTROL: max-age = seconds until advertisement expires
DATE: when reponse was generated
EXT:
LOCATION: URL for UPnP description for root device
SERVER: OS/Version UPNP/1.0 product/version
ST: search target
USN: advertisement UUID</pre>
</td>
</tr>
</tbody>
</table>
&nbsp;

&nbsp;

各HTTP协议头的含义简介：

&nbsp;
<table width="70%" border="0">
<tbody>
<tr>
<td>CACHE-CONTROL</td>
<td>max-age指定通知消息存活时间，如果超过此时间间隔，控制点可以认为设备不存在</td>
</tr>
<tr>
<td>DATE</td>
<td>指定响应生成的时间</td>
</tr>
<tr>
<td>EXT</td>
<td>向控制点确认MAN头域已经被设备理解</td>
</tr>
<tr>
<td>LOCATION</td>
<td>包含根设备描述得URL地址</td>
</tr>
<tr>
<td>SERVER</td>
<td>饱含操作系统名，版本，产品名和产品版本信息</td>
</tr>
<tr>
<td>ST</td>
<td>内容和意义与查询请求的相应字段相同</td>
</tr>
<tr>
<td>USN</td>
<td>表示不同服务的统一服务名，它提供了一种标识出相同类型服务的能力。</td>
</tr>
</tbody>
</table>
&nbsp;

&nbsp;

2、设备通知消息

&nbsp;

在设备加入网络，UPnP发现协议允许设备向控制点广告它的服务。它使用向一个标准地址和端口多址传送发现消息来实现。控制点在此端口上侦听是否有新服务加入系统。为了通知所有设备，一个设备为每个其上的嵌入设备和服务发送一系列相应的发现消息。每个消息也包含它表征设备或服务的特定信息。

**3.1.1 ssdp:alive消息**

在设备加入系统时，它采用多播传送方式发送发现消息，包括告知设备包含的根设备信息，所有嵌入设备以及它包含的服务。每个发现消息包含四个主要对象：

1.  在NT头中包含的潜在搜索目标。
2.  在USN头中包含的复合发现标识
3.  在LOCATION头中关于设备信息的URL地址
4.  在CACHE-CONTROL头中表示的广告消息的合法存在时间。
对于根设备，存在三种发现消息：

&nbsp;
<table width="50%" border="1">
<tbody>
<tr>
<td>NT</td>
<td>USN</td>
</tr>
<tr>
<td>根设备的UUID</td>
<td>根设备的UUID</td>
</tr>
<tr>
<td>设备类型：设备版本</td>
<td>根设备的UUID，设备类型：设备版本</td>
</tr>
<tr>
<td>upnp:rootdevice</td>
<td>根设备的UUID，设备类型和upnp:rootdevice</td>
</tr>
</tbody>
</table>
&nbsp;

对于根设备，存在两种发现消息：

&nbsp;
<table width="50%" border="1">
<tbody>
<tr>
<td>NT</td>
<td>USN</td>
</tr>
<tr>
<td>嵌入设备的UUID</td>
<td>嵌入设备的UUID</td>
</tr>
<tr>
<td>设备类型：设备版本</td>
<td>嵌入设备的UUID，设备类型和设备版本</td>
</tr>
</tbody>
</table>
&nbsp;

对于每个服务：

&nbsp;
<table width="50%" border="1">
<tbody>
<tr>
<td>NT</td>
<td>USN</td>
</tr>
<tr>
<td>服务类型：服务版本</td>
<td>相关设备的UUID，服务类型和服务版本</td>
</tr>
</tbody>
</table>
&nbsp;

如果一个根设备有n个嵌入设备，m个嵌入服务，而且包含k个不同的服务类型，这将会发出3 + 2n + k次请求。这些广告消息像控制点描述了设备的所有信息。这些消息必须作为一系列一起发出，发送的顺序无关紧要，但是不能对单个消息进行刷新或取消的操作。选择一个适当的持续期是在最小化网络通讯和最大化设备状态及时更新之间求得一个平衡，相对较短的持续时间可以保证控制点在牺牲网络流量的前提下获得设备的当前状态；持续期越长可以大大减少设备刷新造成的网络流量。一般而言，设备制造商应该选择一个适当的持续时间值。

由于UDP协议是不可信的，设备应该发送多次设备发现消息。而且为了降低控制点无法收到设备或服务广告消息的可能性，设备应该定期发送它的广告消息。在设备加入网络时，它必须用NOTIFY方法发送一个多播传送请求。NOTIFY方法发送的请求没有回应消息，典型的设备通知消息格式如下：

&nbsp;
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<pre>NOTIFY * HTTP/1.1
HOST: 239.255.255.250:1900CACHE-CONTROL: max-age = seconds until advertisement expires
LOCATION: URL for UPnP description for root device
NT: search target
NTS: ssdp:alive
USN: advertisement UUID</pre>
</td>
</tr>
</tbody>
</table>
&nbsp;

&nbsp;

各HTTP协议头的含义简介：

&nbsp;
<table width="70%" border="0">
<tbody>
<tr>
<td>HOST</td>
<td>设置为协议保留多播地址和端口，必须是239.255.255.250:1900。</td>
</tr>
<tr>
<td>CACHE-CONTROL</td>
<td>max-age指定通知消息存活时间，如果超过此时间间隔，控制点可以认为设备不存在</td>
</tr>
<tr>
<td>LOCATION</td>
<td>包含根设备描述得URL地址</td>
</tr>
<tr>
<td>NT</td>
<td>在此消息中，NT头必须为服务的服务类型。</td>
</tr>
<tr>
<td>NTS</td>
<td>表示通知消息的子类型，必须为ssdp:alive</td>
</tr>
<tr>
<td>USN</td>
<td>表示不同服务的统一服务名，它提供了一种标识出相同类型服务的能力。</td>
</tr>
</tbody>
</table>
&nbsp;

一个发现响应可以包含0个、1个或者多个服务类型实例。为了做出分辨，每个服务发现响应包括一个USN：根设备的标识。在同样的设备里，一个服务类型的多个实例必须用包含USN:ID的服务标识符标识出来。例如，一个灯和电源共用一个开关设备，对于开关服务的查询可能无法分辨出这是用于灯的。UPNP论坛工作组通过定义适当的设备层次以及设备和服务的类型标识分辨出服务的应用程序场景。这么做的缺点是需要依赖设备的描述URL。

**3.1.2 ssdp:byebye消息**

在设备和它的服务将要从网络中卸载时，设备应该对于每个未超期的ssdp:alive消息多播方式传送ssdp:byebye消息。但如果设备突然从网络卸载，它可能来不及发出这个通知消息。因此，发现消息必须在CACHE-CONTROL包含超时值，如果不重新发出广告消息，发现消息最后超时并从控制点的缓存中除去。典型的设备卸载消息格式如下：

&nbsp;
<table width="100%" border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td>
<pre>NOTIFY * HTTP/1.1
HOST: 239.255.255.250:1900NT: search target
NTS: ssdp:byebye
USN: advertisement UUID各HTTP协议头的含义简介：
HOST	设置为协议保留多播地址和端口，必须是239.255.255.250:1900
NT	在此消息中，NT头必须为服务的服务类型。
NTS	表示通知消息的子类型，必须为ssdp:alive
USN	表示不同服务的统一服务名，它提供了一种标识出相同类型服务的能力</pre>
</td>
</tr>
</tbody>
</table>
&nbsp;

&nbsp;

专有设备或服务可以不遵循标准的UPNP模版。但如果设备或服务提供UPNP发现、描述、控制和事件过程的所有对象，它的行为就像一个标准的UPNP设备或服务。为了避免命名冲突，使用专有设备命名时除了UPNP域之外必须包含一个前缀"urn:schemas-upnp-org"。在与标准模版相同时，应该使用整数版本号。但如果与标准模版不同，不可以使用设备复用和徽标。

简单设备发现协议不提供高级的查询功能，也就是说，不能完成某个具有某种服务的设备这样的复合查询。在完成设备或者服务发现之后，控制点可以通过设备或服务描述的URL地址完成更为精确的信息查询。

&nbsp;

&nbsp;

参考资料：

1、SSDP协议原文参考：[http://tools.ietf.org/html/draft-cai-ssdp-v1-03](http://tools.ietf.org/html/draft-cai-ssdp-v1-03)

2、RFC 2616:
关于超文本传输协议（HTTP 1.1）原文IETF的RFC文档 [<span style="color: #5c81a7;">http://www.ietf.org/rfc/rfc2616.txt?number=2616</span>](http://www.ietf.org/rfc/rfc2616.txt?number=2616)

3、UPnP协议框架：

[http://www.upnp.org/specs/arch/UPnP-arch-DeviceArchitecture-v1.0.pdf](http://www.upnp.org/specs/arch/UPnP-arch-DeviceArchitecture-v1.0.pdf)

&nbsp;

转自：[http://www.cnblogs.com/debin/archive/2009/12/01/1614543.html](http://www.cnblogs.com/debin/archive/2009/12/01/1614543.html)