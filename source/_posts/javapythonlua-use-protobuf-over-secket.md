title: "java&python&lua use protobuf over secket"
date: 2014-11-08 19:57:06
tags:
id: 751
comment: false
categories:
  - 学习笔记
---

<pre class="brush:java">public class UDPServer {
	public static void main(String[] args) {

		DatagramSocket aServerSocket;

		try {
			aServerSocket = new DatagramSocket(2222);
			byte[] aReceiveData = new byte[1024 * 1024 * 1];
			DatagramPacket aReceivePacket = new DatagramPacket(aReceiveData,
					aReceiveData.length);
			try {
				aServerSocket.receive(aReceivePacket);
				Person p = Person.newBuilder().setEmail("Alice@example.com")
						.setName("Alice").setId(1000).build();
				byte[] a = p.toByteArray();
				String byteToString = new String(aReceivePacket.getData(), 0,
						aReceivePacket.getLength(), "UTF-8");
				Person aPerson = Person.parseFrom(byteToString
						.getBytes("UTF-8"));
				DatagramPacket sendPacket = new DatagramPacket(a, a.length,
						aReceivePacket.getAddress(), aReceivePacket.getPort());
				aServerSocket.send(sendPacket);
				aPerson.getEmail();
			} catch (IOException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		} catch (SocketException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}</pre>
<pre class="brush:py">#! /usr/bin/python

import person_pb2
import sys
import socket

person = person_pb2.Person()
person.id = 1000
person.name = "Alice"
person.email = "Alice@example.com"
address = ('127.0.0.1', 2222)
s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
s.sendto(person.SerializeToString(), address)
s.close()
</pre>
<pre class="brush:cpp">local person= person_pb.Person()
person.id = 1000
person.name = "Alice"
person.email = "Alice@example.com"
local data = person:SerializeToString()
local sockobj=require("socket")
local sock=sockobj.udp()
sock:sendto(data,"192.168.0.99",2222)
local rdata = sock:receivefrom()
local msg = person_pb.Person()
msg:ParseFromString(rdata)
print(msg.id)
print(msg.name)
print(msg.email)</pre>
&nbsp;