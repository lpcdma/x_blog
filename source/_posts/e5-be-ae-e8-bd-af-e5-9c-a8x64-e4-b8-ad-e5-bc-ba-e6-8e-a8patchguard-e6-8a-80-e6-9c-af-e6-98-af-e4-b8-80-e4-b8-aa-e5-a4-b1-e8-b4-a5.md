title: "微软在X64中强推PatchGuard技术是一个失败"
date: 2014-01-02 17:06:50
tags:
id: 463
comment: false
categories:
  - 学习笔记
---

<span style="font-family: Verdana;">微软在64位系统中强制加入了一项PatchGuard技术，它的作用是防止操作系统自身代码及一些关键数据被Hook或修改，当操作系统检测到这些修改时会引发一个蓝屏错误。</span>

<span style="font-family: Verdana;">微软设计与加入这个功能的初衷有两个：</span>

<span style="font-family: Verdana;">1.提高系统稳定性，据称，微软收到的蓝屏反馈中，有非常高的比率是因为第三方驱动Hook或修改操作系统数据引起。</span>

<span style="font-family: Verdana;">2.防御木马与病毒的入侵。</span>

<span style="font-family: Verdana;">然后我们来分析下这两次目的在PatchGuard技术发布这么些年后的实现情况：</span>

<span style="font-family: Verdana;">1.不可否则，在推出这个技术后，x64的蓝屏率肯定要比x86低很多，但这对安全监控类软件的影响却非常大，在这项技术推出后x64下的安全监控类软件发展出现了一个技术瓶颈，安全开发工程师面临心有余力不足的局面，特别是在推出初期XP x64的时代，微软封杀掉Hook技术后却没有提供其它有效的替代方案，导致杀毒软件在x64上集体退化10年，沦为一个特征扫描器而已，后来在杀软的要求下，微软在Vista以上系统中陆续提供一些监控接口，但因为接口是一致的，导致各大安全软件的监控功能也是基本雷同的，木马病毒攻击变的更加简单。另一方面，其实这也是微软变向地将本该杀软公司要做事拿到自己手里来做。</span>

<span style="font-family: Verdana;">2.在初期x64的确有效抵御了不少从x86移植过来的木马，但木马技术飞速发展，而操作系统却是几年一次更新，技术上根本无法与木马病毒对抗，特别是在目前的环境下远控不再是盗号获取钱财的工具，而慢慢发展成一种政治武器，以至对木马病毒的研发投入急剧增加，致使Stuxnet等这类病毒的产生，近期又出现了TDL4病毒，它拉开了x64 PatchGuard技术的口子，此类病毒必定是越来越多。</span>

<span style="font-family: Verdana;">综上，微软PatchGuard技术的应用，一方面限制了安全软件在x64上的技术进步，另一方面木马却可以绕过此技术为所欲为，毕竟安全软件只能使用正规方法，而木马却可以使用旁门左道，长此以往，Windows安全的出路将在哪里？</span>

转自：[http://www.langouster.com/HTML/128.html](http://www.langouster.com/HTML/128.html)