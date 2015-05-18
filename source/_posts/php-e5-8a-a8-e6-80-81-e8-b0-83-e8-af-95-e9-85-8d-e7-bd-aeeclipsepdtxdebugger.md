title: "PHP动态调试配置(eclipse&PDT&Xdebugger)"
date: 2014-01-12 22:52:44
tags:
id: 481
comment: false
categories:
  - 代码审计
---

1.下载安装eclipse.

[http://www.eclipse.org/downloads/](http://www.eclipse.org/downloads/)

2.安装PDT

[http://projects.eclipse.org/projects/tools.pdt](http://projects.eclipse.org/projects/tools.pdt)

3.下载Xdebugger

[http://xdebug.org/download.php](http://xdebug.org/download.php)

4.在exlipse中配置相关调试所需文件的路径；并配置php.ini

[XDebug]
;zend_extension = "D:\xampp\php\ext\php_xdebug.dll"
;xdebug.profiler_append = 0
;xdebug.profiler_enable = 1
;xdebug.profiler_enable_trigger = 0
;xdebug.profiler_output_dir = "D:\xampp\tmp"
;xdebug.profiler_output_name = "cachegrind.out.%t-%s"
;xdebug.remote_enable = 0
;xdebug.remote_handler = "dbgp"
;xdebug.remote_host = "127.0.0.1"
;xdebug.trace_output_dir = "D:\xampp\tmp"
zend_extension="D:/xampp/php/ext/php_xdebug-2.2.3-5.4-vc9.dll"
xdebug.remote_enable=On
xdebug.remote_host=127.0.0.1
xdebug.remote_port=9000
xdebug.remote_handler="dbgp"

5.创建项目并选择调试项目路径，开始动态调试（特定页面需要设置参数。）