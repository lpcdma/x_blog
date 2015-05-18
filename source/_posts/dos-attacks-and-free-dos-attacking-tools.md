title: "DOS Attacks and Free DOS Attacking Tools"
date: 2013-11-01 14:01:23
tags:
id: 367
comment: false
categories:
  - 工具收集
---

The denial of service (DOS) attack is one of the most powerful attacks used by hackers to harm a company or organization. Don’t confuse a DOS attack with DOS, the disc operating system developed by Microsoft. This attack is one of most dangerous cyber attacks. It causes service outages and the loss of millions, depending on the duration of attack. In past few years, the use of the attack has increased due to the availability of free tools. This tool can be blocked easily by having a good firewall. But a widespread and clever DOS attack can bypass most of the restrictions. In this post, we will see more about the DOS attack, its variants, and the tools that are used to perform the attack. We will also see how to prevent this attack and how not to be the part of this attack.

**What Is a Denial of Service Attack?
**

A DOS attack is an attempt to make a system or server unavailable for legitimate users and, finally, to take the service down. This is achieved by flooding the server’s request queue with fake requests. After this, server will not be able to handle the requests of legitimate users.

In general, there are two forms of the DOS attack. The first form is on that can crash a server. The second form of DOS attack only floods a service.
**DDOS or Distributed Denial of Service Attack
**

This is the complicated but powerful version of DOS attack in which many attacking systems are involved. In DDOS attacks, many computers start performing DOS attacks on the same target server. As the DOS attack is distributed over large group of computers, it is known as a distributed denial of service attack.

To perform a DDOS attack, attackers use a zombie network, which is a group of infected computers on which the attacker has silently installed the DOS attacking tool. Whenever he wants to perform DDOS, he can use all the computers of ZOMBIE network to perform the attack.

In simple words, when a server system is being flooded from fake requests coming from multiple sources (potentially hundreds of thousands), it is known as a DDOS attack. In this case, blocking a single or few IP address does not work. The more members in the zombie network, more powerful the attack it. For creating the zombie network, hackers generally use a Trojan.

There are basically three types of DDOS attacks:

1.  Application-layer DDOS attack
2.  Protocol DOS attack
3.  Volume-based DDOS attack
**Application layer DDOS attack:** Application-layer DDOS attacks are attacks that target Windows, Apache, OpenBSD, or other software vulnerabilities to perform the attack and crash the server.

**Protocol DDOS attack:** A protocol DDOS attacks is a DOS attack on the protocol level. This category includes Synflood, Ping of Death, and more.

**Volume-based DDOS attack**: This type of attack includes ICMP floods, UDP floods, and other kind of floods performed via spoofed packets.

There are many tools available for free that can be used to flood a server and perform an attack. A few tools also support a zombie network to perform DDOS attacks. For this post, we have compiled a few freely available DOS attacking tools.

**Free DOS Attacking Tools**
**
**

**1\. LOIC (Low Orbit Ion Canon)
**

LOIC is one of the most popular DOS attacking tools freely available on the Internet. This tool was used by the popular hackers group Anonymous against many big companies’ networks last year. Anonymous has not only used the tool, but also requested Internet users to join their DDOS attack via IRC.

It can be used simply by a single user to perform a DOS attack on small servers. This tool is really easy to use, even for a beginner. This tool performs a DOS attack by sending UDP, TCP, or HTTP requests to the victim server. You only need to know the URL of IP address of the server and the tool will do the rest.

ou can see the snapshot of the tool above. Enter the URL or IP address and then select the attack parameters. If you are not sure, you can leave the defaults. When you are done with everything, click on the big button saying “IMMA CHARGIN MAH LAZER” and it will start attacking on the target server. In a few seconds, you will see that the website has stopped responding to your requests.

This tool also has a HIVEMIND mode. It lets attacker control remote LOIC systems to perform a DDOS attack. This feature is used to control all other computers in your zombie network. This tool can be used for both DOS attacks and DDOS attacks against any website or server.

The most important thing you should know is that LOIC does nothing to hide your IP address. If you are planning to use LOIC to perform a DOS attack, think again. Using a proxy will not help you because it will hit the proxy server not the target server. So using this tool against a server can create a trouble for you.

Download LOIC Here: http://sourceforge.net/projects/loic/

**2\. XOIC**

XOIC is another nice DOS attacking tool. It performs a DOS attack an any server with an IP address, a user-selected port, and a user-selected protocol. Developers of XOIC claim that XOIC is more powerful than LOIC in many ways. Like LOIC, it comes with an easy-to-use GUI, so a beginner can easily use this tool to perform attacks on other websites or servers.

In general, the tool comes with three attacking modes. The first one, known as test mode, is very basic. The second is normal DOS attack mode. The last one is a DOS attack mode that comes with a TCP/HTTP/UDP/ICMP Message.

It is an effective tool and can be used against small websites. Never try it against your own website. You may end up crashing your own website’s server.

Download XOIC: [http://sourceforge.net/projects/xoic/](http://sourceforge.net/projects/xoic/)

**3\. HULK (HTTP Unbearable Load King)
**

HULK is another nice DOS attacking tool that generates a unique request for each and every generated request to obfuscated traffic at a web server. This tool uses many other techniques to avoid attack detection via known patterns.

It has a list of known user agents to use randomly with requests. It also uses referrer forgery and it can bypass caching engines, thus it directly hits the server’s resource pool.

The developer of the tool tested it on an IIS 7 web server with 4 GB RAM. This tool brought the server down in under one minute.

Download HULK here: [http://packetstormsecurity.com/files/112856/HULK-Http-Unbearable-Load-King.html](http://packetstormsecurity.com/files/112856/HULK-Http-Unbearable-Load-King.html)

**4\. DDOSIM—Layer 7 DDOS Simulator
**

DDOSIM is another popular DOS attacking tool. As the name suggests, it is used to perform DDOS attacks by simulating several zombie hosts. All zombie hosts create full TCP connections to the target server.

This tool is written in C++ and runs on Linux systems.

These are main features of DDOSIM

*   Simulates several zombies in attack
*   Random IP addresses
*   TCP-connection-based attacks
*   Application-layer DDOS attacks
*   HTTP DDoS with valid requests
*   HTTP DDoS with invalid requests (similar to a DC++ attack)
*   SMTP DDoS
*   TCP connection flood on random port
Download DDOSIM here: [http://sourceforge.net/projects/ddosim/](http://sourceforge.net/projects/ddosim/)

Read more about this tool here: http://stormsecurity.wordpress.com/2009/03/03/application-layer-ddos-simulator/

**5\. R-U-Dead-Yet
**

R-U-Dead-Yet is a HTTP post DOS attack tool. For short, it is also known as RUDY. It performs a DOS attack with a long form field submission via the POST method. This tool comes with an interactive console menu. It detects forms on a given URL and lets users select which forms and fields should be used for a POST-based DOS attack.

Download RUDY: [https://code.google.com/p/r-u-dead-yet/](https://code.google.com/p/r-u-dead-yet/)

**6\. Tor’s Hammer
**

Tor’s Hammer is another nice DOS testing tool. It is a slow post tool written in Python. This tool has an extra advantage: It can be run through a TOR network to be anonymous while performing the attack. It is an effective tool that can kill Apache or IIS servers in few seconds.

Download TOR’s Hammer here: [http://packetstormsecurity.com/files/98831/](http://packetstormsecurity.com/files/98831/)

**7\. PyLoris
**

PyLoris is said to be a testing tool for servers. It can be used to perform DOS attacks on a service. This tool can utilize SOCKS proxies and SSL connections to perform a DOS attack on a server. It can target various protocols, including HTTP, FTP, SMTP, IMAP, and Telnet. The latest version of the tool comes with a simple and easy-to-use GUI. Unlike other traditional DOS attacking tools, this tool directly hits the service.

Download PyLoris: [http://sourceforge.net/projects/pyloris/](http://sourceforge.net/projects/pyloris/)

**8\. OWASP DOS HTTP POST
**

It is another nice tool to perform DOS attacks. You can use this tool to check whether your web server is able to defend DOS attack or not. Not only for defense, it can also be used to perform DOS attacks against a website.

Download here: [https://code.google.com/p/owasp-dos-http-post/](https://code.google.com/p/owasp-dos-http-post/)

**9\. DAVOSET
**

DAVOSET is yet another nice tool for performing DDOS attacks. The latest version of the tool has added support for cookies along with many other features. You can download DAVOSET for free from Packetstormsecurity.

Download DavoSET: [http://packetstormsecurity.com/files/123084/DAVOSET-1.1.3.html](http://packetstormsecurity.com/files/123084/DAVOSET-1.1.3.html)

**10\. GoldenEye HTTP Denial Of Service Tool
**

GoldenEye is also a simple but effective DOS attacking tool. It was developed in Python for testing DOS attacks, but people also use it as hacking tool.

Download GoldenEye: [http://packetstormsecurity.com/files/120966/GoldenEye-HTTP-Denial-Of-Service-Tool.html](http://packetstormsecurity.com/files/120966/GoldenEye-HTTP-Denial-Of-Service-Tool.html)

**Detection and Prevention of Denial of Service Attack
**

A DOS attack is very dangerous for an organization, so it is important to know and have a setup for preventing one. Defenses against DOS attacks involve detecting and then blocking fake traffic. A more complex attack is hard to block. But there are a few methods that we can use to block normal DOS attack. The easiest way is to use a firewall with allow and deny rules. In simple cases, attacks come from a small number of IP addresses, so you can detect those IP addresses and then add a block rule in the firewall.

But this method will fail in some cases. We know that a firewall comes at a very deep level inside the network hierarchy, so a large amount of traffic may affect the router before reaching the firewall.

Blackholing and sinkholing are newer approaches. Blackholing detects the fake attacking traffic and sends it to a black hole. Sinkholing routes all traffic to a valid IP address where traffic is analyzed. Here, it rejects back packets.

Clean pipes is another recent method of handling DOS attacks. In this method, all traffic is passed through a cleaning center, where, various methods are performed to filter back traffic. Tata Communications, Verisign, and AT&amp;T are the main providers of this kind of protection.

As an Internet user, you should also take care of your system. Hackers can use your system as a part of their zombie network. So, always try to protect your system. Always keep your system up to date with the latest patches. Install a good antivirus solution. Always take care while installing software. Never download software from un-trusted or unknown sources. Many websites serve malicious software to install Trojans in the systems of innocent users.

**Conclusion
**

In this article, we learned about the denial of service attack and tools used to perform the attack. DOS attacks are used to crash servers and disrupt service. Sony has faced this attack for a long time and lost millions of dollars. It was a big lesson for other companies who rely on server-based income. Every server should set up a way to detect and block DDOS attacks. The availability of free tools makes it easier to perform DOS attack against a website or server. Although most of these tools are only for DOS attacks, a few tools support a zombie network for DDOS attacks. LOIC is the most used and most popular DOS attacking tool. In the past few years, it has been used many times by hackers against big company’s network, so we can never deny the possibility of attack.

So, every company should take care of it and set up good level of protection against DOS attack.