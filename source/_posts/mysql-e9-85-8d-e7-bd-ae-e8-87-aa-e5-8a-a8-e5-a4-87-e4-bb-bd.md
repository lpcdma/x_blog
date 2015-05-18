title: "mysql配置自动备份"
date: 2013-06-15 04:11:00
tags:
id: 265
comment: false
categories:
  - 编程学习
---

mysqlbak.sh
<pre class="brush:shell">date="$(date +"%Y-%m-%d")"
mysqldump --set-gtid-purged=OFF -u lpcdma -h x.x.x.x -p'123456' lpcdma | gzip -9 &gt; "/backup/databackup-$date.gz"</pre>
/etc/crontab
<pre class="brush:shell">SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
01 1 * * * root /usr/sbin/mysqlback.sh</pre>
测试
<pre class="brush:shell">[root@xserver backup]# sh /usr/sbin/mysqlback.sh
date="$(date +"%Y-%m-%d")"
mysqldump --set-gtid-purged=OFF -u lpcdma -h x.x.x.x -p'123456' lpcdma | gzip -9 &gt; "/backup/databackup-$date.gz"
Warning: Using a password on the command line interface can be insecure.
[root@xserver backup]# ls
databackup-2013-06-15.gz
[root@xserver backup]# gzip -d databackup-2013-06-15.gz 
[root@xserver backup]# ls -al
total 1152
drwxrwxrwx 2 root root    4096 Jun 15 04:09 .
drwxr-xr-x 3 root root    4096 Jun 15 03:50 ..
-rw-r--r-- 1 root root 1165467 Jun 15 03:56 databackup-2013-06-15</pre>
&nbsp;