title: "sun官方源安装jdk"
date: 2014-04-10 14:56:48
tags:
id: 629
comment: false
categories:
  - 编程学习
---

1.) If you’ve already installed OpenJDK (Or in ubantu generally it is already installed). Remove it by running this command: `sudo apt-get purge openjdk*`

2.) Then:

    sudo add-apt-repository ppa:webupd8team/java`</pre>
    Then update:
    <pre>`sudo apt-get update`</pre>
    3.) Select which version you want To install Oracle Java 8:
    <pre>`sudo apt-get install oracle-java8-installer`</pre>
    To install Oracle Java 7:
    <pre>`sudo apt-get install oracle-java7-installer`</pre>
    To install the Java 6:
    <pre>`sudo apt-get install oracle-java6-installer