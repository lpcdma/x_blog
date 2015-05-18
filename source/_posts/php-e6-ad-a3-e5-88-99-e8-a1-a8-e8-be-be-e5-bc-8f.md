title: "PHP正则表达式"
date: 2013-06-11 00:08:57
tags:
id: 224
comment: false
categories:
  - 学习笔记
---

PHP的三种正则引擎是“preg”,"ereg"和“mb_ereg”。

preg引擎提供的函数，使用的是NFA引擎。在速度方面都强于其余两者。

PHP preg的正则流派
<pre class="brush:php">字符缩略表示法
\a [\b] \e \f \n \r \t \octal \xhex \x{hex} \cchar
字符组及相关结构
字符组: […] [^…] 
几乎任何字符: 点号
Unicode混合序列: \X
字符组缩略表示法: \w \d \s \W \D \S 
Unicode属性和区块: \p{Prop} \p{Prop}
单个字节: \C
锚点及其他零长度断言
行/字符串起始位置: ^ \A
行/字符串结束位置: $ \z \Z
当前匹配起始位置: \G
单词分界符: \b \B
环视结构: (?=…) (?!…) (?&lt;=…) (?&lt;!…)
注释及模式修饰符
模式修饰符: (?mods-mods) 容许出现的模式:x s m i X U
模式修饰范围: (?mods-mods…)
注释: (?#…)
分组及捕获
捕获型括号: (…) \1 \2…
命名捕获 (?P&lt;name&gt;…) (?P=name)
仅分组的括号: (?:…)
固话分组: (?&gt;…)
多选结构: |
递归: (?R) (?num) (?P&gt;name)
条件判断: (? if then|else)
匹配优先量词: * + ? {n} {n,} {x,y}
忽略优先量词: *? +? ?? {n?} {n,}? {x,y}?
占有优先量词: *+ ++ ?+ (n)+ {n,}+ {x,y}+
文字范围: \Q…\E

PHP Preg函数概览
preg_math 测试正则表达式能否在字符串中找到匹配，并提取数据
preg_match_all 从字符串中提取数据
preg_replace 在字符串的副本中替换匹配的文本
preg_replace_callback 对字符串中的每处匹配文本叫用处理函数
preg_split 将字符串切分为子串组
preg_grep 选出数组中能/不能由表达式匹配的元素
preg_quote 转义字符串中的正则表达式元字符

/* 测试HTML tag中是否&lt;table&gt; tag*/
if (preg_match('/^&lt;table\b/i',$tag))

模式修饰符
i 忽略大小写
m 增强的行锚点模式 //注入使用\n编码绕过
s 点号通配模式
x 宽松排列和注释模式
u 以UTF-8读取正则表达式和目标字符串
X 启用PCRE"额外功能(extra stuff)"
e 将replacement作为PHP代码(只用于preg_replace)
S 启用PCRE的"study"优化尝试
U 交换『*』和『*?』的匹配优先含义
A 将整个匹配尝试锚定在起始位置
D 『$』只能匹配EOS,而不是EOS之前的换行符

/*从字符串中取出HTML title*/
if (preg_match('{&lt;title&gt;(.*?)&lt;/title&gt;}si',$html,$captures))</pre>
&nbsp;

&nbsp;