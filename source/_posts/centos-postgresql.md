title: "centos postgresql"
date: 2012-09-26 22:11:15
tags:
id: 117
comment: false
---

安装

yum install postgresql postgresql-server

初始化

/etc/init.d/postgresql-9.1 initdb       ///var/lib/pgsql/9.1/data/ == null

设置postgres密码

# passwd postgres

Changing password for user postgres.

New password: ！！！！！

Retype new password: ！！！！

passwd: all authentication tokens updated successfully.

启动服务

/etc/init.d/postgresql-9.1 start

登录

#su postgres

psql (8.4.13, server 9.1.6)

WARNING: psql version 8.4, server version 9.1.

Some psql features might not work. Type "help" for help.

创建数据库

postgres=# create database msf owner postgres;

CREATE DATABASE

修改密码   //这一步有点疑惑，但是必须做

postgres=# alter user postgres with password '！！！！！！';

ALTER ROLE

postgres=# q

&nbsp;

&nbsp;