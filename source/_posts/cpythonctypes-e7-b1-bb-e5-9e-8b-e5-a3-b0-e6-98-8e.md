title: "C&Python&ctypes类型声明"
date: 2013-11-25 10:05:06
tags:
id: 411
comment: false
categories:
  - 学习笔记
---

<pre class="brush:cpp">-----------------------------------------------------------------
C Type                       Python Type              ctypes Type
-----------------------------------------------------------------
char                         string                   c_char
wchar_t                      Unicode string           c_wchar
char                         int/long                 c_byte
char                         int/long                 c_ubyte
short                        int/long                 c_short
unsigned short               int/long                 c_ushort
int                          int/long                 C_int
unsigned int                 int/long                 c_int
long                         int/long                 c_long
unsigned long                int/long                 c_ulong
long long                    int/long                 c_longlong
float                        float                    c_float
double                       float                    c_double
char *(NULL terminated)      string or none           c_char_p
wchar_t *(NULL terminated)   unicode or none          c_wchar_p
void *                       int/long rog none        c_woid_p

</pre>