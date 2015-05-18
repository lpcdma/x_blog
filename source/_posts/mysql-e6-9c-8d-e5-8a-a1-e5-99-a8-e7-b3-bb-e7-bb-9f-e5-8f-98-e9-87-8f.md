title: "mysql 服务器系统变量"
date: 2012-06-16 22:42:34
tags:
id: 70
comment: false
---

sql-shell&gt;select @@version_compile_os 获取数据库服务器类型

sql-shell&gt; select @@datadir 获取当前数据库目录

sql-shell&gt; select @@hostname 获取数据服务器名称

sql-shell&gt; select @@version 获取数据版本

sql-shell&gt; select user() 获取数据库当前用户(登陆用户)

sql-shell&gt; select database() 获取当前数据库名称

sql-shell&gt;select @@plugin_dir 获取mysql用户自定义函数目录

&nbsp;

&nbsp;

[转载](http://exploit-db.blogcn.com/articles/mysql-%e6%9c%8d%e5%8a%a1%e5%99%a8%e7%b3%bb%e7%bb%9f%e5%8f%98%e9%87%8f%e5%87%bd%e6%95%b0.html#more-50)